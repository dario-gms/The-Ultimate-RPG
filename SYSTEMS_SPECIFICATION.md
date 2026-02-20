# The Eternal Throne: Systems Specification Document

**Version 1.0** | Technical Reference for Implementation

---

## Table of Contents

1. [Relationship System Specification](#relationship-system-specification)
2. [Morality Axes Specification](#morality-axes-specification)
3. [Economic System Specification](#economic-system-specification)
4. [Judicial System Specification](#judicial-system-specification)
5. [Military System Specification](#military-system-specification)
6. [Consequence Tracking Specification](#consequence-tracking-specification)
7. [NPC AI Behavior Specification](#npc-ai-behavior-specification)

---

## Relationship System Specification

### Data Structure

```cpp
struct RelationshipData {
    // Relationship ID
    string character_id;
    
    // Seven-dimensional relationship tracking
    float rapport;           // -100 to +100 (personal warmth)
    float trust;             // -100 to +100 (willingness to be vulnerable)
    float alignment;         // -10 to +10 (morality axis similarity)
    float rivalry;           // -100 to +100 (competitive/adversarial)
    float obligation;        // -100 to +100 (debts and promises)
    float romantic_tension;  // 0-100 (physical/emotional attraction)
    float authority_respect; // -100 to +100 (fear/respect of power)
    
    // Relationship status
    enum Status {
        UNKNOWN,
        ACQUAINTANCE,
        FRIEND,
        CLOSE_FRIEND,
        ROMANTIC_INTEREST,
        PARTNER,
        MARRIED,
        RIVAL,
        ENEMY,
        BETRAYED
    };
    Status current_status;
    
    // History
    int first_meeting_day;
    int last_interaction_day;
    vector<DialogueEvent> conversation_history;
    vector<ConflictEvent> conflict_history;
    
    // Flags
    set<string> relationship_flags;
    bool is_married;
    bool has_children;
    vector<string> children_ids;
    int jealousy_level; // 0-100
};
```

### Relationship Dynamics

#### Rapport System

**Definition**: Personal warmth and friendship affection

**Increases When**:
- Spending quality time together (+5 per interaction)
- Helping with personal quests (+10-20)
- Agreeing with their values (+5 per dialogue choice)
- Gift giving (+10 per valuable gift)
- Saving their life (+30)
- Keeping promises (+10)
- Personal moments (vulnerable conversation) (+15)

**Decreases When**:
- Ignoring their requests (-5)
- Breaking promises (-20)
- Disagreeing consistently (-5 per conflict)
- Making them wait too long (-2 per in-game month)
- Insulting them directly (-15)
- Prioritizing someone else over them (-10)
- Public humiliation (-25)

**Effects**:
- At 75+: They initiate positive interactions, provide advice
- At 50-75: Friendly, willing to help
- At 25-50: Neutral, transactional
- At 0-25: Cold, minimal engagement
- At -25-0: Strained, conflicts emerge
- At -75+: Openly hostile

#### Trust System

**Definition**: Willingness to be vulnerable and share secrets

**Builds Through**:
- Keeping confidences (they tell secret, you don't share) (+20)
- Vulnerability reciprocation (you share something personal) (+15)
- Long-term reliability (consistent behavior over months) (+10 per major promise kept)
- Support in difficult time (+25)
- Not exploiting leverage against them (+15)

**Broken By**:
- Betraying a confidence (-50)
- Blackmail or coercion (-75)
- Siding with their enemy (-40)
- Lying directly (-30)
- Exploiting their vulnerability (-50)

**Effects**:
- At 75+: They confide major secrets, personal goals, fears
- At 50-75: Share moderate secrets and concerns
- At 0-50: Guard personal information
- At -50+: Active distrust, assume you'll betray them

#### Alignment System

**Definition**: Compatibility on morality axes

**Calculation**:
```
alignment = (1 - abs(player_justice - npc_justice)/20) * 10
            + similar calculations for other axes
average across all four axes
Result: -10 (complete opposition) to +10 (perfect alignment)
```

**Effects**:
- Aligned decisions (+relationship with that NPC)
- Opposed decisions (-relationship with that NPC)
- Extreme opposition (constant conflict): NPC may abandon you

#### Jealousy System

**Definition**: Resentment toward rivals or perceived replacement

**Triggers**:
- Spending significant time with romantic rival (+10 per major interaction)
- Marrying someone else while in relationship with them (+50 immediately)
- Promoting rival to position of power they wanted (+15)
- Public displays of affection with someone else (+20)
- Neglect (long time without interaction) (+5 per month)

**Effects**:
- At 25-50: Jealous comments, pouting behavior
- At 50-75: Threatening to leave you, petty sabotage
- At 75+: Active hostility toward rival, ultimatum decisions

**Resolution**:
- Reassurance and time together (-10 per major interaction)
- Ending rival relationship (-30 immediately)
- Marriage (if romantic) (-50, converts to commitment)

### Romance Progression System

#### Stage 1: Attraction Recognition

**Conditions**:
- Rapport >= 40
- Romantic_tension >= 30
- Alignment >= 5 on at least one major axis
- Available (not married to someone else)

**Triggers**:
- Moment of vulnerability followed by support
- Rescue or protection moment
- Shared personal secret
- Physical proximity and eye contact (described in text)
- Declaration of interest (dialogue option or NPC initiated)

**Duration**: Variable (can happen same day or take weeks)

#### Stage 2: Romantic Relationship

**Characteristics**:
- Partner becomes companion (joins player's party)
- Special dialogue options unlock
- Physical intimacy described (non-explicit)
- Romantic tension continues to build or decrease based on interaction
- Partner may express concerns about player's other relationships
- Personal quests may intersect with relationship

**Potential Outcomes**:
- Deepening (partner suggests commitment)
- Stagnation (relationship plateaus, may drift apart)
- Conflict (disagreement on values, time commitment, goals)
- Cooling (one or both parties lose romantic interest)

**Cooling Mechanics**:
- Long separations (-5 per week apart)
- Consistent value disagreement (-10 per major conflict)
- Romantic rival appearing (-20)
- Partner's goals and player's goals diverging (-15)
- Player's other romantic interests (-10 per each)

#### Stage 3: Commitment/Marriage

**Requirements**:
- Romantic relationship >= 2 months
- Rapport >= 75
- Trust >= 60
- Jealousy < 30

**Player Actions**:
- Propose marriage (dialogue option)
- Arrange diplomat marriage (with council approval)

**NPC Initiation**:
- If rapport >= 85, partner may propose

**Marriage Effects**:
- Partner's faction becomes allied
- Shared wealth/property
- Partner joins council
- Partner moves into player's residence
- Spouse title reflects player's title (Queen, Lord, etc.)
- Sexual relationship (abstracted)
- Pregnancy possible

**Marriage Complications**:

| Complication | Trigger | Resolution |
|-------------|---------|-----------|
| Jealousy of job | Player spends more time ruling than with spouse | Quality time and reassurance |
| Ideological conflict | Major decision contradicts spouse's values | Compromise or accept resentment |
| Family pressure | Spouse's family makes demands | Negotiate with family or refuse |
| Career ambition | Spouse wants independent power | Support ambitions or refuse |
| Infidelity temptation | Attractive alternative appears | Dialogue choice to remain faithful |

#### Stage 4: Divorce

**Conditions**:
- Rapport drops below 30
- Trust drops below 20
- Jealousy exceeds 75
- Continuous major conflicts without resolution

**Initiation**:
- Player can always initiate
- NPC initiates if rapport/trust too low
- Spouse's faction may pressure for divorce if alliance no longer beneficial

**Consequences**:
- Alliance with spouse's faction potentially lost
- Property division dispute
- Alimony/financial settlement
- Custody of children becomes issue
- Reputation damage (viewed as disloyal or unstable)
- Spouse may become enemy or cool acquaintance

### Children System

#### Conception

**Requirements**:
- Married to partner
- Spouse rapport >= 70
- Player not in active combat situation for extended period
- Spouse remains in player's location

**Chance Per Month**:
- Base: 5% per month
- Increased by: Quality time (+2%), Shared values (+1%)
- Decreased by: Constant travel (-3%), Conflict (-5%)

#### Childhood Development (years 0-16)

**Personality Development**:
- Inherits traits from both parents (genetic traits)
- Develops based on parenting choices:
  - Discipline vs. freedom
  - Education focus (military, scholarly, diplomatic, religious)
  - Exposure to politics (early or delayed)

**Parenting Interactions**:
- Teach combat skills (military focus)
- Teach diplomacy (political focus)
- Teach scholarly subjects (magical/knowledge focus)
- Teach morality (values education)
- Quality time (bonding)

**Parenting Effects**:
- Frequent parenting (+personality traits reflecting parent's values)
- Neglect (-personality development, child resentful)
- Harsh discipline (+obedience, -happiness, possible rebellion later)
- Permissive parenting (+happiness, -obedience)

#### Coming of Age (years 16-18)

**Personality Locks**:
- Child's core personality becomes fixed
- Educational path set
- Relationship with player stabilized

**Marriage Arrangements**:
- Player can arrange marriage to ally's child
- Grants alliance with that faction
- Child has opinion on arrangement (approval/disapproval)
- Can be arranged before child is of age (creates obligation)

#### Adulthood (18+)

**Autonomy**:
- Child becomes NPC with independent goals
- Can pursue personal quests
- May take positions (military, diplomatic, religious)
- Can inherit titles upon parent's death

**Succession**:
- Eldest child typically inherits
- Can be overridden by will or political decision
- Multiple heirs create succession dispute
- Child's values affect how they rule

---

## Morality Axes Specification

### Four-Axis Model

#### Axis 1: Justice ↔ Pragmatism

**Justice Side (positive direction)**:
- Belief in objective moral law
- Consistency and precedent matter
- Innocent until proven guilty
- Punishment should fit crime
- Rules apply equally

**Pragmatism Side (negative direction)**:
- Results matter more than process
- Flexibility in rules for outcomes
- Ends justify means
- Situational ethics
- Practical solutions over ideals

**Range**: -10 (Pure Pragmatism) to +10 (Pure Justice)

**Decision Examples**:

| Decision | Justice Choice | Pragmatic Choice |
|----------|----------------|------------------|
| Trial verdict | Evidence-based verdict | Verdict that prevents riot |
| Punishment | Sentence fits crime | Harsh if example needed |
| Law enforcement | Consistent enforcement | Selective enforcement |
| Broken contract | Enforce contract as written | Renegotiate for benefit |

#### Axis 2: Honor ↔ Manipulation

**Honor Side (positive direction)**:
- Direct communication
- Keep promises
- Personal integrity valued
- Truth over flattery
- Transparency

**Manipulation Side (negative direction)**:
- Deception is acceptable
- Say what people want to hear
- Promises are flexible
- Use people's weaknesses
- Strategic information control

**Range**: -10 (Pure Manipulation) to +10 (Pure Honor)

**Decision Examples**:

| Decision | Honor Choice | Manipulation Choice |
|----------|--------------|---------------------|
| Promise fulfillment | Keep promise even if costly | Break if no one finds out |
| Negotiation | State true position | Offer false terms to get better deal |
| Secrets | Don't use secrets for advantage | Blackmail with secret |
| Flattery | Be honest about impressions | Flatter to gain support |

#### Axis 3: Freedom ↔ Control

**Freedom Side (positive direction)**:
- Individual liberty valued
- Minimal government intrusion
- Personal choice respected
- Dissent allowed
- Diversity accepted

**Control Side (negative direction)**:
- Order through authority
- Collective good over individual wants
- Hierarchy enforced
- Dissent suppressed
- Conformity expected

**Range**: -10 (Total Control) to +10 (Total Freedom)

**Decision Examples**:

| Decision | Freedom Choice | Control Choice |
|----------|----------------|-----------------|
| Conscription | Volunteer army | Mandatory military service |
| Religion | Religious freedom | State religion enforced |
| Speech | Allow all speech | Suppress seditious speech |
| Licensing | No requirements | Strict licensing for trades |

#### Axis 4: Idealism ↔ Realpolitik

**Idealism Side (positive direction)**:
- Beliefs matter
- Long-term principles valued
- Help others despite cost
- Change unjust systems
- Vision matters

**Realpolitik Side (negative direction)**:
- Power matters most
- Short-term advantage prioritized
- Help only if beneficial
- Accept unjust systems that benefit you
- Pragmatic goals over vision

**Range**: -10 (Pure Realpolitik) to +10 (Pure Idealism)

**Decision Examples**:

| Decision | Idealism Choice | Realpolitik Choice |
|----------|-----------------|-------------------|
| Refugee crisis | Accept refugees despite cost | Refuse refugees, secure borders |
| War goal | Fight just war with restraint | Use any means for victory |
| Ally choice | Support moral ally | Support strongest ally |
| Reform | Implement ideal but costly reform | Accept imperfect status quo |

### Player Tracking

#### Axis Position Tracking

```cpp
struct MoralityState {
    float justice;        // -10 to +10
    float honor;          // -10 to +10
    float freedom;        // -10 to +10
    float idealism;       // -10 to +10
    
    int decisions_made;
    int major_decisions;
    
    // Historical positions (for character arc tracking)
    vector<MoralityState> history;
};
```

#### Decision Impact

**Minor Decisions** (small flags, routine choices): ±0.5 per axis

**Standard Decisions** (meaningful choice with consequence): ±1-2 per axis

**Major Decisions** (game-changing choice with lasting impact): ±3-5 per axis

**Example Decision**:
- Decision: Execute innocent peasant to prevent riot
  - Pragmatism +2 (practical outcome prioritized)
  - Justice -3 (innocent person punished)
  - Honor -2 (betrayal of justice principle)

### NPC Reaction to Axes

#### Compatibility Calculation

```
compatibility_score = 0
for each axis in [justice, honor, freedom, idealism]:
    player_axis = player.morality[axis]
    npc_axis = npc.morality[axis]
    distance = abs(player_axis - npc_axis)
    if distance <= 5:
        compatibility_score += (10 - distance)
    else:
        compatibility_score -= (distance - 10)

return compatibility_score / 4  // Average across axes
```

#### Response Matrix

| Compatibility | NPC Response |
|--------------|-------------|
| +20 to +40 | Strong approval, will follow your lead |
| +10 to +20 | Approval, generally supports you |
| 0 to +10 | Mild approval, willing to work with you |
| -10 to 0 | Neutral, transactional |
| -20 to -10 | Disapproval, reluctant to support |
| -30 to -20 | Strong disapproval, will oppose you |
| -40+ | Implacable opposition, betrayal likely |

---

## Economic System Specification

### Regional Prosperity Model

```cpp
struct RegionalEconomy {
    // Base production
    float agricultural_output;    // bushels per season
    float resource_extraction;    // ore, lumber, etc.
    float manufacturing_output;   // goods produced
    float trade_value;            // from trade routes
    
    // Derived values
    float total_wealth;           // GPM (gold per month)
    float tax_base;               // how much can be taxed
    
    // Modifiers
    float security_modifier;      // reduced by war/banditry
    float prosperity_level;       // affects prices and migration
    float inflation_factor;
    
    // Population
    int population;
    float employment_rate;
    float happiness;
};
```

### Income Sources

#### Trade Route Income

**Mechanic**:
- Each trade route connects two cities
- Each route generates 100 GPM base
- Modifiers:
  - Route security: Unsecured = 50% penalty; secured = 150% bonus
  - Route length: Longer routes = higher income but higher risk
  - Faction relations: Hostile faction blocking route = 0 income
  - Infrastructure: Better roads = higher income (200 GPM)

#### Taxation

**Formula**:
```
Effective_Tax_Income = (Region_Wealth × Tax_Rate) × (1 - Evasion_Rate) × Collection_Quality
```

**Tax Rates**:
- 0%: No revenue, but 50% happiness bonus
- 5%: Minimal revenue, no morale penalty
- 10%: Standard rate, no morale effect
- 15%: High rate, -10 happiness, 5% evasion
- 20%: Very high, -20 happiness, 15% evasion
- 25%: Extreme, -30 happiness, 30% evasion

**Collection Quality**:
- Corruption level affects how much gets collected
- Honest collectors: 90% collection rate
- Moderately corrupt: 75% collection rate
- Highly corrupt: 50% collection rate

**Evasion**:
- Calculated as: 1 + (Tax_Rate / 10) - (Prosperity / 20)
- Higher taxes and lower prosperity = more evasion

#### Property Income

**Tavern**: 20 GPM base
- Modifier: Region prosperity (±10%)
- Upgrade: +10 GPM, requires 100 gold

**Shop**: 30 GPM base
- Modifier: Competition from other shops
- Upgrade: +15 GPM

**Estate**: 50 GPM base
- Modifier: Agricultural productivity
- Upgrade: +25 GPM

**Stronghold**: Military only, no income (costs 20 GPM to maintain)

### Inflation Model

**Formula**:
```
inflation_rate = (player_monthly_spending / regional_wealth) × 0.1
```

**Effects**:
- Prices increase by inflation rate per month
- Wages increase (reduces profit from employees)
- Previously saved gold decreases in value
- Encourages spending on infrastructure

**Cap**: Inflation maxes out at 5% per month (catastrophic)

### Economic Crisis

**Triggers**:
- Taxation >20% for 3+ months
- Military spending >50% income for 3+ months
- War in region for 3+ months
- Natural disaster (plague, drought)

**Symptoms**:
- Prosperity drops 50%
- Tax revenue drops 30%
- Population emigration (+2% per month)
- Military recruitment reduced 50%
- Banditry increases (trade routes less safe)

**Recovery**:
- Reduce taxation to encourage prosperity recovery
- Invest in infrastructure (roads, markets, temples)
- Military victories improve morale
- Time (minimum 6 months of peace)

---

## Judicial System Specification

### Trial Framework

#### Trial Structure

```cpp
struct Trial {
    string trial_id;
    string accused_id;
    string accuser_id;
    
    enum ChargeType {
        MURDER,
        THEFT,
        TREASON,
        HERESY,
        ADULTERY,  // Noble scandal
        DERELICTION, // Military failure
        BREACH_OF_CONTRACT,
        ASSAULT
    };
    ChargeType charge;
    
    // Evidence and argument
    string accusation_description;
    string defense_description;
    vector<Witness> witnesses;
    vector<Evidence> physical_evidence;
    
    // Context
    string precedent_trials[];
    int public_opinion_toward_accused;
    int public_opinion_toward_accuser;
    
    enum Verdict {
        INNOCENT,
        GUILTY,
        CONDITIONAL
    };
    Verdict verdict;
    string punishment_description;
};
```

#### Evidence System

**Types of Evidence**:
- **Witness testimony**: Reliability varies (hostile witness = unreliable)
- **Physical evidence**: Weapon, stolen goods, documents
- **Motive**: Does accused have reason to commit crime?
- **Opportunity**: Was accused in location at time?
- **Precedent**: Similar cases and how they were judged

**Evidence Weight**:
- Each evidence piece has 0-100 weight
- Hostile witness: 40 weight
- Neutral witness: 70 weight
- Friendly witness (unreliable): 30 weight
- Physical evidence: 80 weight
- Motive proof: 60 weight

**Evidence Sum**:
```
prosecution_case = sum of evidence weights supporting guilt
defense_case = sum of evidence weights supporting innocence
```

### Verdict Types

#### Innocent

**Consequences**:
- Accused is freed
- Accuser loses face (reputation -10)
- If accuser is powerful noble: Risk of bloodfeud
- Public opinion shifts toward accused

#### Guilty

**Punishment Options**:
- **Execution**: Remove character from game entirely
- **Exile**: Character banished from region
- **Fines**: Monetary penalty
- **Servitude**: Work as slave/servant for period
- **Imprisonment**: Lock up for period
- **Mutilation**: Remove hand/tongue (permanent disability)
- **Public shaming**: Reputation damage, but alive

**Execution Consequences**:
- Family resentment (-50 relationship with all family members)
- Potential bloodfeud (family seeks revenge)
- Public opinion split:
  - If deserved: +justice reputation
  - If seen as unfair: -tyranny reputation
- If executed person has allies: Possible rebellion

#### Conditional Verdicts

**Format**: Guilty of X but with exceptions

**Examples**:
- "Guilty of theft but exile instead of hanging"
- "Guilty of murder but acceptable due to self-defense"
- "Guilty but family remains honored"

**Effects**:
- Allows nuance without perfect innocence or harsh guilt
- Can satisfy both prosecution and defense partially
- Precedent for future similar trials

### Verdict Impact

#### On Accused

```
reputation_change = {
    INNOCENT: +20 (vindication)
    GUILTY_LIGHT: -30 (minor punishment)
    GUILTY_MODERATE: -50 (standard punishment)
    GUILTY_SEVERE: -80 (harsh punishment)
}

relationship_change = {
    Friends: -20 (if guilty), +30 (if innocent)
    Enemies: +30 (if guilty), -20 (if innocent)
}
```

#### On Accuser

```
If verdict is INNOCENT:
    accuser_reputation: -30
    accuser_relationship_with_player: -20
    
If verdict is GUILTY:
    accuser_reputation: +15
    accuser_relationship_with_player: +10
```

#### On Factions

- If accused is faction member: Affects faction relations
- If verdict is seen as unjust: Faction relations decrease
- If verdict protects faction: Faction relations increase

#### On General Population

```
public_perception = {
    if verdict matches evidence clearly:
        justice_reputation: +10
    if verdict contradicts evidence (obviously unjust):
        tyranny_reputation: +20
    
    consistency (if verdict matches previous similar verdicts):
        predictability: +5 per match
}
```

---

## Military System Specification

### Army Composition

```cpp
struct Army {
    string army_id;
    string commander_id;
    Location current_location;
    
    // Unit composition
    int infantry_count;        // standard soldiers
    int cavalry_count;         // mounted fighters
    int archer_count;          // ranged units
    int siege_equipment;       // 0-5 (used for sieges)
    
    // Army state
    int current_morale;        // 0-100
    int current_fatigue;       // 0-100 (0=rested, 100=exhausted)
    int supply_level;          // months of food available
    int casualties;            // permanent losses
    
    enum Status {
        MUSTERED,
        MARCHING,
        BESIEGING,
        RESTING,
        SCATTERED
    };
    Status status;
};
```

### Recruitment

**Sources**:
- Conscription: Forces all able-bodied males into service
  - Fast recruitment (weeks)
  - Causes unhappiness
  - Units have lower morale
- Volunteers: People enlist willingly
  - Slower recruitment (months)
  - Better morale
  - Limited by population willingness
- Mercenaries: Hire foreign soldiers
  - Very fast (weeks)
  - Expensive (costs gold per month)
  - No loyalty (will abandon if not paid)
- Professional soldiers: Standing army
  - Costs 20 GPM per 100 soldiers
  - Always available
  - Better training

**Recruitment Limits**:
```
conscription_limit = population * 0.15
volunteer_limit = population * 0.05 * popularity_factor
mercenary_limit = unlimited (if can afford)
```

### Movement & Logistics

**Supply System**:
- Each soldier requires 1 supply per month
- Supply consumed faster during rapid marching
- No supply = morale drops 10 per month, casualties increase

**Movement Speed**:
- Normal march: 50 km per month
- Forced march: 100 km per month (requires double supply)
- Besieging location: 0 km (stationary)
- Retreating: 150 km per month (panic flight)

**Engagement Decision**:
When armies meet, player chooses:
- **Attack**: Initiate battle immediately
- **Ambush**: Attack from hidden position (+20% damage first round)
- **Siege**: Surround location, starve enemy supplies
- **Retreat**: Flee to safety (-10 morale, losses possible)
- **Parley**: Negotiate (offer peace, vassalization, tribute)

### Battle Mechanics

**Turn-Based Tactical Combat**:
- All units act simultaneously each turn
- Player commands army formations and tactics
- Battle duration: 10-20 turns
- Casualties calculated at end of each turn

**Damage Calculation**:
```
damage_dealt = (attacker_unit_count × unit_damage) × tactic_modifier × terrain_modifier
```

**Tactic Modifiers**:
- **Aggressive**: +30% damage, -20% defense
- **Defensive**: +30% defense, -20% damage
- **Balanced**: No modifier
- **Skirmish**: +10% mobility, -15% damage
- **Hammer & anvil**: +50% damage if executed perfectly, -40% if enemy predicts

**Terrain Modifiers**:
- Plains: No modifier
- Forest: +30% defense, -20% ranged damage
- Hills: +20% morale if defending, ranged advantage
- Fortification: +50% defense (only when defending location)
- River: -20% mobility, blocks cavalry advantage

**Morale Effects**:
- High morale (75+): No penalty
- Medium morale (50-75): -10% damage, easier to route
- Low morale (25-50): -20% damage, units surrender more easily
- Broken morale (0-25): -50% damage, units flee

**Battle Resolution**:
- Battle ends when:
  - One side routed (morale drops below 0)
  - One side casualty rate exceeds 50%
  - Siege complete (starved army surrenders)
  - Player retreats

### Army Morale

**Morale Increases**:
- Victory: +20 morale
- Resting in friendly territory: +5 per month
- Commander has positive trait: +10
- Good supply: +2 per month
- Pay bonus (gold given): +5

**Morale Decreases**:
- Defeat: -30 morale
- Desertion: -5 per unit that deserts
- No supply: -10 per month
- Forced march: -5
- Casualties: -1 per casualty rate percentage
- Commander has negative trait: -15

**Morale Consequences**:
- Below 30: Desertion begins (1-2% per month)
- Below 0: Army routs and scatters

---

## Consequence Tracking Specification

### Consequence Log Structure

```cpp
struct ConsequenceEvent {
    string consequence_id;
    string action_id;  // Original action that caused consequence
    int trigger_day;
    
    enum ConsequenceType {
        PERSONAL,      // Relationship change
        POLITICAL,     // Faction/power change
        ECONOMIC,      // Wealth/trade change
        MILITARY,      // Army strength change
        NARRATIVE      // Story event triggered
    };
    ConsequenceType type;
    
    string affected_entity_id;
    string affected_entity_type;  // NPC, Faction, Location, etc.
    
    float impact_magnitude;        // 0-100 (severity)
    vector<string> secondary_consequences;
    
    bool has_resolved;
    int resolution_day;
};
```

### Consequence Cascade Examples

#### Example 1: Execution of Noble

**Primary Action**:
```
Day 47: Execute Lord Harren for treason
```

**Immediate Consequences** (Day 47):
- Lord_Harren_House faction relations: -50
- Harren family members: -70 relationship
- Population fear: +20
- Rebel sentiment: +10
- Trial precedent set: Nobles can be executed for treason

**Secondary Consequences** (Day 60):
- Harren House raises 500-unit army
- 3 minor lords decrease relations with player (-20 each)
- Merchants speculate on instability (prices increase 10%)
- Assassination attempts begin (1-2 per month)

**Tertiary Consequences** (Day 120):
- Rebellion becomes open warfare
- If rebellion grows: Civil war possible
- If rebellion suppressed: Harden opposition (+20% future rebellion risk)
- If negotiated: Peace but Harren House remains hostile

**Quaternary Consequences** (Day 365+):
- Entire regional political landscape shifted
- Other nobles remember: Lessons learned for own behavior

### Consequence Resolution Tracking

For each consequence, track:

| Attribute | Purpose |
|-----------|---------|
| Status | Active, Mitigated, Resolved, Dormant |
| Severity | Current impact magnitude |
| Stakeholders | Who is affected |
| Resolution paths | How it can be resolved |
| Deadline | Any time pressure |

### Persistent Consequence System

**Consequences never truly disappear**; instead they:
1. **Resolve**: Situation stabilized, but history remains
2. **Mitigate**: Reduce impact but not eliminate
3. **Dormant**: Temporarily inactive, can reactivate
4. **Transform**: Change into different consequence

**Example**:
- Civil war might resolve through victory, but resulting resentment is persistent
- Executed noble's family remains hostile even after 100 years
- Broken treaty can be renegotiated, but trust is permanently reduced

---

## NPC AI Behavior Specification

### NPC Decision-Making

```cpp
struct NPCBrain {
    // Goals
    Goal primary_goal;
    vector<Goal> secondary_goals;
    
    // Values
    float morality_axes[4];
    vector<string> personality_traits;
    
    // Situation awareness
    vector<Fact> known_facts;
    vector<NPC*> relationships;
    float self_assessment; // How capable they think they are
    
    // Decision variables
    float risk_tolerance;
    float ambition_level;
    float loyalty_to_player;
    float emotional_state; // Happy, angry, fearful, excited
};

struct Goal {
    string goal_description;
    float priority;  // 0-100
    float progress;  // 0-100
    int deadline;    // Days remaining, or -1 for no deadline
};
```

### Goal Evaluation

**Each month**, NPCs evaluate their situation:

1. **Check primary goal progress**
   - Is progress being made?
   - What obstacles exist?
   - Can player help or hinder?

2. **Evaluate current situation**
   - Am I safe?
   - Am I prosperous?
   - Are my relationships stable?

3. **Decide on actions**
   - Continue current plan
   - Pursue opportunity
   - Defend against threat
   - Seek player assistance
   - Betray player if beneficial

### Companion Behavior

**Companions in party**:
- Follow player's commands
- Comment on decisions
- Assist in conversations based on their values
- Question orders that conflict with their values

**Out of party**:
- Pursue personal goals
- Maintain relationships
- Take jobs or positions
- Develop their own story arcs

### Romance Companion Behavior

**In relationship**:
- Express affection
- Become jealous of rivals
- Offer advice (biased toward their values)
- Pressure player for commitment
- Express concerns about player's decisions

**Specific behaviors**:
- If player makes decision violating core values: "I can't support this"
- If player spends extended time away: "I miss you"
- If player starts new romance: "Who is that?" (jealousy)
- If player ignores: Initiate heart-to-heart or breakup conversation

---

## Appendix: Data Formats

### JSON Save Format Example

```json
{
  "relationship": {
    "character_id": "npc_001_sarah",
    "rapport": 75,
    "trust": 60,
    "alignment": 7,
    "rivalry": 0,
    "obligation": -10,
    "romantic_tension": 45,
    "authority_respect": 50,
    "status": "ROMANTIC_INTEREST",
    "is_married": false,
    "jealousy_level": 20,
    "conversation_history": [
      {
        "day": 10,
        "dialogue_id": "sarah_first_meet",
        "choices_made": [0, 1],
        "flags_set": ["met_sarah", "heard_her_story"]
      }
    ]
  },
  "morality_state": {
    "justice": 6,
    "honor": 4,
    "freedom": 8,
    "idealism": 5
  },
  "regional_economy": {
    "region_id": "north_kingdom",
    "agricultural_output": 5000,
    "total_wealth": 1500,
    "tax_rate": 0.12,
    "population": 50000,
    "prosperity_level": 0.85,
    "inflation_factor": 1.02
  }
}
```

---

**Document Version**: 1.0  
**Last Updated**: February 2026  
**Status**: Technical Specification Complete
