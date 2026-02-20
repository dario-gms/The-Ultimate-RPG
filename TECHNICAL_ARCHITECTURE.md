# The Eternal Throne: Technical Architecture Document

**Version 1.0** | Engine & Code Structure Specifications

---

## Table of Contents

1. [Executive Technical Summary](#executive-technical-summary)
2. [Architecture Overview](#architecture-overview)
3. [Core Systems Architecture](#core-systems-architecture)
4. [Engine Requirements](#engine-requirements)
5. [Code Structure](#code-structure)
6. [Data Management](#data-management)
7. [Performance Optimization](#performance-optimization)
8. [Testing Strategy](#testing-strategy)

---

## Executive Technical Summary

**The Eternal Throne** requires a sophisticated game engine capable of:

- **Complex State Management**: Tracking 1000+ persistent flags and relationships across game world
- **Background Simulation**: Political and economic AI running independent of player action
- **Reactive Narrative**: Dynamic dialogue and events responding to world state
- **Large-Scale Simulation**: Multiple factions with autonomous decision-making
- **Save/Load Integrity**: Complete serialization of game state without corruption

**Recommended Platforms**:
1. **Unreal Engine 5** (C++) - Best for large-scale systems and performance
2. **Custom C++ Engine** - Maximum control over specific systems
3. **Unity** (C#) - Viable if optimized carefully for background systems

**Estimated Code Size**: 500K-1M lines of code (depending on approach)

**Development Team Size**: 20-40 people (10-15 systems engineers, 15-25 content creators)

**Development Timeline**: 24-36 months

---

## Architecture Overview

### Layered Architecture Diagram

```
┌─────────────────────────────────────────────┐
│      User Interface Layer                    │
│  (Dialogue, HUD, Maps, Menus)               │
└─────────────────────────────────────────────┘
                      ↑↓
┌─────────────────────────────────────────────┐
│      Gameplay Systems Layer                  │
│  (Combat, Travel, Relationships, Trials)    │
└─────────────────────────────────────────────┘
                      ↑↓
┌─────────────────────────────────────────────┐
│      Simulation Systems Layer                │
│  (Political AI, Economics, Consequences)    │
└─────────────────────────────────────────────┘
                      ↑↓
┌─────────────────────────────────────────────┐
│      Persistence Layer                       │
│  (Game State, Save/Load, Database)          │
└─────────────────────────────────────────────┘
                      ↑↓
┌─────────────────────────────────────────────┐
│      Engine Layer (Unreal/Custom)            │
│  (Graphics, Physics, Input, File I/O)       │
└─────────────────────────────────────────────┘
```

### System Integration Points

```
┌─────────────────┐
│  Dialogue       │←─────────────┐
│  System         │              │
└─────────────────┘              │
         ↓                        │
┌─────────────────┐         ┌──────────────┐
│  Consequence    │────────→│ World State  │
│  Engine         │         │  Database    │
└─────────────────┘         └──────────────┘
         ↑                        ↓
┌─────────────────┐         ┌──────────────┐
│  Political AI   │←────────│  Event       │
│  & Factions     │         │  Trigger     │
└─────────────────┘         └──────────────┘
```

---

## Core Systems Architecture

### 1. Game State Manager

**Responsibility**: Central manager for all persistent game state

```cpp
class GameStateManager : public Singleton {
public:
    // World state
    class WorldState* world_state;
    class TimeSystem* time_system;
    
    // Core systems
    class CharacterSystem* character_system;
    class RelationshipSystem* relationship_system;
    class PoliticalSystem* political_system;
    class EconomicSystem* economic_system;
    class JudicialSystem* judicial_system;
    class MilitarySystem* military_system;
    class DialogueSystem* dialogue_system;
    
    // Managers
    class ConsequenceEngine* consequence_engine;
    class EventDispatcher* event_dispatcher;
    class SaveGameManager* save_manager;
    
    // Core loops
    void UpdateGameState(float delta_time);
    void ProcessDayTick();
    void ProcessMonthTick();
    void ProcessYearTick();
    void OnEventTriggered(Event event);
};
```

**Key Responsibilities**:
- Initialize all systems in correct order
- Maintain single source of truth for world state
- Dispatch events between systems
- Manage game time progression
- Handle save/load operations

### 2. Character & Relationship System

```cpp
class CharacterSystem {
private:
    map<string, Character*> characters;
    class RelationshipGraph relationship_graph;
    
public:
    Character* GetCharacter(string character_id);
    Relationship* GetRelationship(string char_a, string char_b);
    void UpdateCharacterState(Character* character, float delta_time);
    void AddCharacterFlag(string char_id, string flag);
    bool HasCharacterFlag(string char_id, string flag);
};

class RelationshipSystem {
private:
    map<pair<string,string>, RelationshipData> relationships;
    
public:
    void ModifyRelationship(string char_a, string char_b, 
                           string relationship_aspect, float delta);
    float GetRelationshipValue(string char_a, string char_b);
    void UpdateRelationshipStatus(string char_a, string char_b);
    void OnCharacterDecision(string character_id, Decision decision);
    void ProcessRelationshipDecay(); // Relationships change over time
};
```

**Relationship Update Process**:

```
For each NPC pair:
  1. Check if relationship was affected by recent events
  2. Apply event modifiers
  3. Check for dialogue opportunities
  4. Check for status transitions (acquaintance→friend, etc.)
  5. Update relationship decay (absence, time apart)
  6. Trigger callbacks if relationship threshold crossed
```

### 3. Dialogue System

```cpp
class DialogueSystem : public IEventListener {
private:
    class DialogueDatabase dialogue_db;
    map<string, DialogueState*> active_dialogues;
    
public:
    DialogueState* StartDialogue(string npc_id, string dialogue_id);
    vector<DialogueOption> GetAvailableOptions(DialogueState* state);
    void SelectDialogueOption(DialogueState* state, int option_index);
    
private:
    bool IsDialogueOptionAvailable(DialogueOption option);
    void ApplyDialogueConsequences(DialogueState* state);
    void GenerateDynamicDialogueResponse(DialogueState* state);
};

struct DialogueOption {
    string text;
    vector<string> required_flags;    // Must have these flags
    vector<string> forbidden_flags;   // Must NOT have these flags
    float morality_alignment[4];      // Player's axis position after choice
    string consequence_flag;          // Flag to set if chosen
    float relationship_impact;        // Impact on relationship
};
```

**Dialogue Processing**:

```
Player initiates dialogue with NPC
  ↓
Load dialogue tree for NPC
  ↓
Get NPC's current emotional state (based on recent events)
  ↓
Filter dialogue options based on:
    - Player flags & history
    - Relationship status
    - Morality axis alignment
    - NPC knowledge (what NPC knows about player)
  ↓
Present filtered options to player
  ↓
Player selects option
  ↓
Apply consequences:
    - Set flags
    - Modify relationship
    - Trigger events
    - Generate follow-up dialogue
  ↓
Close dialogue / Continue
```

### 4. Consequence Engine

**The most critical system**: All decisions feed into consequence tracking

```cpp
class ConsequenceEngine {
private:
    vector<ConsequenceEvent> consequence_log;
    multimap<string, ConsequenceEvent*> event_dependency_graph;
    
public:
    void LogAction(string action_id, Player player_decision);
    void TriggerConsequence(ConsequenceEvent consequence);
    void UpdateConsequenceStatus();
    vector<ConsequenceEvent> GetActiveConsequences();
    
private:
    void CalculateImpact(ConsequenceEvent event);
    void TriggerSecondaryConsequences(ConsequenceEvent primary);
    void ApplyConsequenceModifiers(ConsequenceEvent& event);
};

struct ConsequenceEvent {
    string consequence_id;
    int trigger_day;
    ConsequenceType type;          // PERSONAL, POLITICAL, ECONOMIC, etc.
    vector<string> affected_npcs;
    vector<string> affected_factions;
    map<string, float> impact_map; // What property changed and by how much
    
    // Cascade tracking
    vector<string> parent_consequences;
    vector<string> child_consequences;
    bool has_resolved;
};
```

**Consequence Propagation Example**:

```
Player executes noble (consequence_001)
  ├─ Immediate: Set flags
  │   ├─ "noble_executed"
  │   ├─ "family_hostile"
  │   └─ "fear_of_tyranny"
  │
  ├─ Personal Impact (consequence_002)
  │   └─ Affected NPCs: Noble's family members
  │       └─ Relationship change: -70 with all
  │       └─ New consequence: "family_seeks_revenge"
  │
  ├─ Political Impact (consequence_003)
  │   └─ Affected Factions: Noble's faction
  │       └─ Faction relations: -50
  │       └─ New consequence: "faction_raises_army"
  │
  └─ Systemic Impact (consequence_004)
      └─ Affected NPCs: Other nobles
          └─ Relationship change: -20 (fear of execution)
          └─ New consequence: "noble_loyalty_questioned"

[Days later]
Family_seeks_revenge triggers assassination attempt
Faction_raises_army triggers military threat
Noble_loyalty_questioned triggers defection of minor lords
```

### 5. Political System (Faction AI)

```cpp
class PoliticalSystem {
private:
    map<string, Faction*> factions;
    class DiplomacyGraph diplomacy;
    
public:
    void UpdateFactionAI();
    void ProcessDiplomaticAction(string faction_id);
    Diplomacy* GetDiplomacy(string faction_a, string faction_b);
    
private:
    Decision EvaluateFactionDecision(Faction* faction);
    float EvaluateOption(Faction* faction, Decision option);
};

struct Faction {
    string faction_id;
    string leader_id;
    Goal primary_goal;
    vector<Goal> secondary_goals;
    
    // Resources
    float military_strength;
    float economic_wealth;
    float political_influence;
    
    // AI state
    map<string, NPC*> members;
    map<string, Diplomacy> relationships;
    vector<string> controlled_territories;
    
    Decision MakeDecision(GameState game_state);
};
```

**Faction Decision Process**:

```cpp
void Faction::MakeMonthlyDecision() {
    // Evaluate current situation
    float goal_progress = EvaluateGoalProgress();
    float current_threat_level = EvaluateThreatLevel();
    float economic_stability = EvaluateEconomicStatus();
    
    // Generate options
    vector<Decision> options;
    options.push_back(ContinueCurrentPlan());
    options.push_back(SeekNewAlly());
    options.push_back(ExpandTerritory());
    options.push_back(DefendTerritory());
    
    // Score each option
    map<Decision, float> option_scores;
    for (auto option : options) {
        float score = 0;
        score += ScoreGoalProgress(option, primary_goal) * 0.6;
        score += ScoreGoalProgress(option, secondary_goals) * 0.3;
        score += RiskEvaluation(option) * 0.1;
        option_scores[option] = score;
    }
    
    // Choose highest-scoring option
    Decision best_option = MaxElement(option_scores);
    ExecuteDecision(best_option);
}
```

### 6. Economic System

```cpp
class EconomicSystem {
private:
    map<string, RegionalEconomy> regions;
    map<string, TradeRoute> trade_routes;
    map<string, PlayerEconomy> player_assets;
    
public:
    void UpdateRegionalEconomy(string region_id);
    void ProcessTradeRoutes();
    void ProcessTaxation();
    void CheckForEconomicCrisis();
};

void EconomicSystem::ProcessMonthlyUpdate() {
    // 1. Calculate regional output
    for (auto& region : regions) {
        region.agricultural_output = CalculateHarvest();
        region.resource_extraction = CalculateResourcesExtracted();
        region.trade_income = CalculateTradeIncome();
        region.total_wealth = agricultural + extraction + trade;
    }
    
    // 2. Apply modifiers
    for (auto& region : regions) {
        ApplySecurityModifier(region);
        ApplyProsperityModifier(region);
        ApplyInflationModifier(region);
    }
    
    // 3. Collect taxes
    for (auto& region : regions) {
        float tax_income = region.total_wealth * tax_rate * (1 - evasion_rate);
        AddToPlayerWealth(tax_income);
        UpdatePopulationHappiness(region, tax_rate);
    }
    
    // 4. Check for crisis
    for (auto& region : regions) {
        if (region.prosperity < 0.3f) {
            TriggerEconomicCrisis(region);
        }
    }
}
```

### 7. Save/Load System

```cpp
class SaveGameManager {
private:
    class Serializer serializer;
    
public:
    void SaveGame(string save_filename);
    void LoadGame(string save_filename);
    vector<string> GetSaveFiles();
    
private:
    void SerializeGameState(GameState state, FileStream& stream);
    GameState DeserializeGameState(FileStream& stream);
    
    // Validation
    bool ValidateSaveIntegrity(GameState state);
    bool CheckVersionCompatibility(int save_version);
};
```

**Save File Structure**:

```
Save File (Binary Format)
├─ Header
│   ├─ Version number (1.0)
│   ├─ Timestamp
│   └─ Checksum
├─ Metadata
│   ├─ Player character name
│   ├─ Current location
│   ├─ Game time (days elapsed)
│   └─ Play duration (hours)
├─ World State
│   ├─ Characters (serialized)
│   ├─ Relationships (serialized)
│   ├─ Flags (compressed bitset)
│   └─ Location states
├─ Systems State
│   ├─ Factions and diplomacy
│   ├─ Economic data
│   ├─ Military units
│   └─ Active consequences
└─ Dialogue History
    └─ Conversation records
```

---

## Engine Requirements

### Unreal Engine 5 Approach

**Advantages**:
- Robust animation system
- Strong AI behavior tree system
- Good performance for large worlds
- Built-in dialogue system (Unreal Dialogue System)
- Marketplace assets available

**Core Systems**:
```cpp
// UE5 Classes to extend
ACharacter → CustomCharacter
AGameMode → CustomGameMode
UGameInstance → CustomGameInstance
UActorComponent → RelationshipComponent, ConsequenceComponent
UInterface → ICharacterBehavior, IDialogueNPC
```

**Key UE5 Systems Used**:
- **Behavior Trees**: NPC decision-making and goal pursuit
- **Environment Query System (EQS)**: NPC location decisions
- **Blueprints**: Rapid iteration on dialogue and events
- **Widget Blueprint**: UI for relationships, council, trials
- **Data Tables**: Dialogue, items, stats storage

### Custom C++ Engine Approach

**Advantages**:
- Maximum control over memory and performance
- Exactly tailored to project needs
- Smaller file size

**Disadvantages**:
- Longer initial development
- More systems to build (graphics, physics, audio)
- Higher technical risk

**Minimum Required Modules**:
- Graphics engine (3D rendering)
- Input system
- Audio system
- File I/O and serialization
- Physics (simplified)
- Scripting engine (Lua for dialogue)

### Performance Targets

| System | Target FPS | Target Latency |
|--------|-----------|-----------------|
| Exploration/Dialogue | 60 FPS | <16ms per frame |
| Combat (Tactical) | 60 FPS | <16ms per frame |
| World Map | 30-60 FPS | <33ms per frame |
| Dialogue System | N/A | <100ms response |
| AI Update Cycle | N/A | <200ms per NPC |

---

## Code Structure

### Directory Organization

```
ProjectRoot/
├── Source/
│   ├── Core/
│   │   ├── GameStateManager.h/cpp
│   │   ├── TimeSystem.h/cpp
│   │   └── EventDispatcher.h/cpp
│   │
│   ├── Characters/
│   │   ├── Character.h/cpp
│   │   ├── RelationshipSystem.h/cpp
│   │   ├── CharacterAI.h/cpp
│   │   └── CompanionSystem.h/cpp
│   │
│   ├── Systems/
│   │   ├── Political/
│   │   │   ├── FactionSystem.h/cpp
│   │   │   ├── DiplomacySystem.h/cpp
│   │   │   └── FactionAI.h/cpp
│   │   ├── Economic/
│   │   │   ├── EconomicSystem.h/cpp
│   │   │   ├── TradeSystem.h/cpp
│   │   │   └── TaxationSystem.h/cpp
│   │   ├── Military/
│   │   │   ├── ArmySystem.h/cpp
│   │   │   ├── BattleSystem.h/cpp
│   │   │   └── StrategicMap.h/cpp
│   │   ├── Judicial/
│   │   │   ├── TrialSystem.h/cpp
│   │   │   ├── VerdictEngine.h/cpp
│   │   │   └── JudgmentSystem.h/cpp
│   │   └── Dialogue/
│   │       ├── DialogueSystem.h/cpp
│   │       ├── DialogueParser.h/cpp
│   │       └── DialogueDatabase.h/cpp
│   │
│   ├── Simulation/
│   │   ├── ConsequenceEngine.h/cpp
│   │   ├── EventSystem.h/cpp
│   │   └── SimulationClock.h/cpp
│   │
│   ├── Persistence/
│   │   ├── SaveGameManager.h/cpp
│   │   ├── Serializer.h/cpp
│   │   └── DatabaseConnection.h/cpp
│   │
│   ├── UI/
│   │   ├── DialogueUI.h/cpp
│   │   ├── HUD.h/cpp
│   │   ├── MenuUI.h/cpp
│   │   └── RelationshipUI.h/cpp
│   │
│   └── Utilities/
│       ├── StringUtils.h/cpp
│       ├── MathUtils.h/cpp
│       ├── Logger.h/cpp
│       └── ConfigManager.h/cpp
│
├── Content/
│   ├── Characters/
│   ├── Locations/
│   ├── Dialogue/
│   └── Assets/
│
├── Config/
│   ├── GameConfig.json
│   ├── CharacterData.json
│   └── DialogueDatabase.json
│
└── Tests/
    ├── Unit/
    ├── Integration/
    └── SystemTests/
```

---

## Data Management

### World State Database

**Technology**: SQLite (persistent) + In-Memory Database (runtime)

```sql
-- Simplified schema
CREATE TABLE characters (
    character_id TEXT PRIMARY KEY,
    name TEXT,
    location_id TEXT,
    morality_justice INT,
    morality_honor INT,
    morality_freedom INT,
    morality_idealism INT,
    current_day INT
);

CREATE TABLE relationships (
    character_a TEXT,
    character_b TEXT,
    rapport FLOAT,
    trust FLOAT,
    alignment FLOAT,
    romantic_tension FLOAT,
    status TEXT,
    PRIMARY KEY (character_a, character_b)
);

CREATE TABLE events (
    event_id TEXT PRIMARY KEY,
    event_type TEXT,
    trigger_day INT,
    affected_entities TEXT, -- JSON array
    impact_magnitude FLOAT,
    resolved BOOLEAN
);

CREATE TABLE flags (
    flag_name TEXT PRIMARY KEY,
    flag_value BOOLEAN,
    set_day INT
);
```

### In-Memory State

For performance, most state is kept in-memory during gameplay:

```cpp
// Runtime game state (serialized on save)
struct GameState {
    // Players
    Player player_character;
    vector<NPC> all_npcs;
    
    // World
    vector<Location> locations;
    vector<Faction> factions;
    
    // Tracking
    set<string> global_flags;
    vector<ConsequenceEvent> consequences;
    map<pair<string,string>, RelationshipData> relationships;
    
    // Derived data (rebuilt from source)
    FactionGraph faction_graph;
    EventDependencyGraph event_graph;
    EconomySimulation economy;
};
```

---

## Performance Optimization

### Critical Optimization Areas

#### 1. Relationship Update Optimization

**Naive approach**: Update every relationship every frame (O(n²) complexity)

**Optimized approach**:
```cpp
void RelationshipSystem::UpdateRelationships() {
    // Only update relationships affected by recent events
    set<string> changed_npcs;
    for (auto event : recent_events) {
        for (auto npc_id : event.affected_npcs) {
            changed_npcs.insert(npc_id);
        }
    }
    
    // Only update relationships involving changed NPCs
    for (auto npc_a : changed_npcs) {
        for (auto npc_b : npc_a.relationships) {
            UpdateSingleRelationship(npc_a, npc_b);
        }
    }
}
```

**Result**: Instead of O(n²) updates per frame, only O(n) affected relationships

#### 2. Faction AI Optimization

**Naive approach**: Every faction makes decision every frame

**Optimized approach**:
```cpp
void PoliticalSystem::UpdateFactions() {
    // Each faction decides monthly, not per frame
    if (current_day % 30 == 0) {
        for (auto faction : factions) {
            faction.MakeMonthlyDecision();
        }
    }
    
    // Execute already-decided actions
    for (auto faction : factions) {
        faction.ExecutePendingActions();
    }
}
```

#### 3. Dialogue System Optimization

**Caching**:
```cpp
class DialogueSystem {
private:
    map<string, DialogueOptionList> dialogue_cache;
    
public:
    vector<DialogueOption> GetAvailableOptions(string npc_id) {
        // Check if already calculated this session
        if (dialogue_cache.count(npc_id)) {
            return dialogue_cache[npc_id];
        }
        
        // Calculate only if not cached
        auto options = CalculateAvailableOptions(npc_id);
        dialogue_cache[npc_id] = options;
        return options;
    }
};
```

#### 4. Economic Simulation Optimization

**Spatial partitioning**:
- Only simulate regions that have changed
- Cache prosperity calculations
- Use approximation for distant regions

### Memory Management

**Estimated Memory Usage**:
- Character data: 50KB per NPC × 300 NPCs = 15 MB
- Relationships: 5KB per relationship × 45K relationships = 225 MB
- World state flags: 1 bit per flag × 1000 flags = 125 bytes
- Consequences: 200 bytes × 500 active = 100 KB
- **Total runtime**: ~250 MB (reasonable for modern systems)

**Optimization techniques**:
- Use object pooling for frequently created objects
- Lazy load dialogue and content as needed
- Stream location data
- Clear unused cache periodically

---

## Testing Strategy

### Unit Tests

```cpp
// Example: Test relationship changes
TEST(RelationshipSystem, RapportIncreasesWhenHeeping) {
    Character npc = CreateTestCharacter();
    Player player = CreateTestPlayer();
    
    RelationshipData rel = GetRelationship(player, npc);
    float initial_rapport = rel.rapport;
    
    HelpNPCWithQuest(&npc);
    
    rel = GetRelationship(player, npc);
    EXPECT_GT(rel.rapport, initial_rapport);
    EXPECT_EQ(rel.rapport, initial_rapport + 10);
}

// Example: Test consequence cascade
TEST(ConsequenceEngine, ExecutionTriggersRebelion) {
    ExecuteNPC(noble);
    
    // Check immediate consequences
    EXPECT_TRUE(HasGlobalFlag("noble_executed"));
    EXPECT_LT(GetFactionRelations(noble.faction, player), -30);
    
    // Simulate 30 days
    SimulateDays(30);
    
    // Check secondary consequences
    EXPECT_TRUE(HasGlobalFlag("rebellion_started"));
    EXPECT_GT(GetUnrestLevel(region), 50);
}
```

### Integration Tests

- Test complete loops (dialogue → decision → consequence)
- Test system interactions (economic collapse → military weakness)
- Test save/load integrity

### Performance Tests

- Measure frame time with 300 NPCs and 1000 active consequences
- Profile relationship update performance
- Test faction AI decision time

### Playtesting Focus

- **Balance**: Is consequence severity appropriate?
- **Fairness**: Are morality axis changes consistent with decisions?
- **Clarity**: Do players understand relationship mechanics?
- **Emergence**: Do systems create interesting, unexpected situations?

---

## Appendix: Reference Implementations

### Consequence Application Example

```cpp
void ApplyConsequence(ConsequenceEvent consequence) {
    switch (consequence.type) {
        case PERSONAL: {
            for (auto npc_id : consequence.affected_npcs) {
                NPC* npc = GetNPC(npc_id);
                npc->relationship_with_player.rapport += consequence.impact_magnitude;
                npc->AddFlag(consequence.consequence_id);
            }
            break;
        }
        case POLITICAL: {
            for (auto faction_id : consequence.affected_factions) {
                Faction* faction = GetFaction(faction_id);
                faction->relations_with_player -= consequence.impact_magnitude;
                faction->TriggerReaction();
            }
            break;
        }
        case ECONOMIC: {
            player_wealth -= consequence.impact_magnitude * 1000;
            break;
        }
        case MILITARY: {
            player_army->morale -= consequence.impact_magnitude;
            break;
        }
    }
    
    // Check for secondary consequences
    for (auto secondary_id : consequence.secondary_consequences) {
        ApplyConsequence(GetConsequence(secondary_id));
    }
}
```

### NPC Decision-Making Example

```cpp
Decision NPC::MakeDecision() {
    vector<Decision> options = GenerateDecisionOptions();
    float best_score = -INFINITY;
    Decision best_decision;
    
    for (auto option : options) {
        float score = 0;
        
        // Primary goal alignment
        score += ScoreTowardGoal(option, primary_goal) * 0.7f;
        
        // Value alignment with player (if companion)
        if (is_companion) {
            float value_diff = abs(player.morality_sum - my_morality_sum);
            score += (20 - value_diff) * 0.2f;
        }
        
        // Risk assessment
        if (my_risk_tolerance == HIGH) {
            score += RandomDecision() * 0.1f;
        }
        
        // Loyalty to player
        score += (my_loyalty_to_player / 100) * 0.1f;
        
        if (score > best_score) {
            best_score = score;
            best_decision = option;
        }
    }
    
    return best_decision;
}
```

---

**Document Version**: 1.0  
**Last Updated**: February 2026  
**Status**: Architectural Design Complete
