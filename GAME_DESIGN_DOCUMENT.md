# The Eternal Throne: Complete Game Design Document

**Version 1.0** | Last Updated: February 2026

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Design Pillars](#design-pillars)
3. [Core Gameplay Loops](#core-gameplay-loops)
4. [System Architecture](#system-architecture)
5. [Consequence System](#consequence-system)
6. [Morality Framework](#morality-framework)
7. [Romance & Relationship System](#romance--relationship-system)
8. [Political Simulation](#political-simulation)
9. [World Design](#world-design)
10. [End-Game Systems](#end-game-systems)
11. [Technical Considerations](#technical-considerations)

---

## Project Overview

### Vision Statement

> **The Eternal Throne** is a narrative-driven RPG where personal stories, political power, and moral choices are inextricably linked. Every decision permanently alters the game world, creating emergent narratives through systemic interaction rather than predetermined plot rails.

### Design Goals

1. **Narrative Integration**: Cinematic storytelling that emerges from systemic consequence, not just scripted scenes
2. **Meaningful Agency**: Player choices have cascading, irreversible impacts across multiple systems
3. **Emergent Complexity**: Simple rules create complex, unexpected outcomes
4. **Dynamic World State**: NPCs, factions, and civilizations evolve independently based on player actions and AI goals
5. **Replayability through Consequence**: Each playthrough creates a fundamentally different world

### Player Fantasy

- Rule a kingdom with real political consequences
- Build romances and dynasties that feel authentic
- Make morally complex decisions without "correct" answers
- See the world transformed by accumulated choices
- Play across generations through heir succession

---

## Design Pillars

### Pillar 1: Cinematic Narrative Depth

**Inspiration**: Baldur's Gate 3, Witcher 3, Disco Elysium

#### Objectives
- Create strong personality-driven characters with independent goals
- Implement complex romantic arcs with genuine conflict
- Present morally ambiguous choices with no objectively "correct" answer
- Ensure dialogues have measurable, world-altering consequences

#### Core Mechanics

**Character Personality System**
- Each major NPC has:
  - Defined values (aligned to the four morality axes)
  - Personality traits (ambitious, loyal, idealistic, pragmatic, etc.)
  - Personal goals and secrets
  - Dynamic personality shifts based on player choices
  - Conditional dialogue trees based on relationship history

**Relationship Depth System**
- Relationships are not simple "approval ratings"
- Instead, track:
  - **Rapport**: Personal warmth and friendship
  - **Trust**: Willingness to share secrets/vulnerability
  - **Alignment**: Shared values on the morality axes
  - **Rivalry**: Competitive or adversarial dynamics
  - **Obligation**: Debts and promises made
  - **Attraction**: Romantic potential (separate from rapport)
  - **Fear/Respect**: Authority-based relationships

**Companion Arc System**
- Each companion has a personal narrative thread independent of the main plot
- Arcs can:
  - **Flourish**: Companion achieves their goal with player support
  - **Compromise**: Achieve goal partially, with consequences
  - **Fail**: Companion's goal becomes impossible; relationship fractures
  - **Betray**: Companion acts against player based on conflicting values
  - **Transform**: Companion changes goals based on player influence
  - **Elevate**: Companion achieves political power through player support

**Dialogue Consequence System**
- Major dialogue choices are irreversible
- Dialogue flags persist throughout game (no hidden stat resets)
- NPCs reference past conversations and hold grudges/bonds
- Dialogue creates branching narrative opportunities later
- Some dialogue options are gated by:
  - Relationship levels
  - Morality axis alignment
  - Previous choices made
  - Hidden secrets discovered

---

### Pillar 2: Life Simulation & Personal Power

**Inspiration**: Fable 3, The Sims 4, CK3 Dynasty Mode

#### Objectives
- Allow meaningful relationships with any significant NPC
- Implement marriage, children, and family dynamics
- Create personal economic power through property and investment
- Build personal reputation that affects all interactions

#### Core Mechanics

**Relationship Foundation System**
- The player can pursue romantic relationships with compatible NPCs
- Relationships have emotional depth, not just mechanical benefits
- Romances can lead to marriage or remain as affairs depending on choice
- Relationships have genuine conflicts (ideological disputes, jealousy, career pressures)

**Family & Succession System**
- Player can marry and produce heirs
- Children inherit:
  - **Genetic traits**: Physical appearance, predispositions
  - **Social traits**: Values, personality developed through parenting choices
  - **Political leverage**: Claims to power, alliances through their lineage
  - **Economic inheritance**: Titles, properties, wealth
- Player can shape heir development through:
  - Education choices (military, scholarly, diplomatic training)
  - Personal involvement and quality time
  - Exposure to political intrigue
  - Marriage arrangement for strategic alliance

**Personal Reputation System**
- Player builds regional reputation through:
  - **Public Decisions**: Laws enacted, judgments made, military victories
  - **Personal Conduct**: Generosity, cruelty, honesty, deception
  - **Relationship Status**: Marriage alliances, family prominence
  - **Economic Power**: Wealth accumulated, investments made
- Reputation affects:
  - NPC disposition toward player
  - Prices for goods and services
  - Ability to command loyalty
  - Eligibility for marriages
  - Merchant and craftsperson discounts

**Property & Economic System**
- Player can purchase:
  - **Housing**: Residences affecting quality of life and guest impressions
  - **Businesses**: Taverns, shops generating passive income
  - **Estates**: Land with economic output (crops, resources)
  - **Strongholds**: Military locations providing strategic advantages
- Each property:
  - Generates income based on region and management
  - Requires maintenance and improvements
  - Can be upgraded to provide bonuses
  - Can be lost through economic collapse or conquest
- Economic simulation:
  - Simple but intelligible tax system
  - Inflation based on player spending
  - Regional prosperity affects prices
  - Trade routes create economic corridors

**Dynamic Morale & Loyalty**
- Your allies, companions, and subjects have morale affected by:
  - Your decisions and fairness
  - Economic conditions in regions they control
  - Military victories and defeats
  - Personal relationship quality
  - Betrayal of promises or values
- Low morale leads to:
  - Reduced effectiveness in service
  - Possibility of defection
  - Complaints and complaints to other NPCs
  - Potential rebellion if severe

---

### Pillar 3: Governance & Judgement

**Inspiration**: Dragon Age: Inquisition, CK3, Divinity Original Sin 2 - Fort Joy decisions

#### Objectives
- Give player real political power with genuine consequences
- Create a council system with competing agendas
- Implement judicial system with long-term political impact
- Build military strategy with resource constraints

#### Core Mechanics

**Council System**
- Player leads a council of major NPCs, each with:
  - **Domain**: Military, Economics, Espionage, Religion, Justice
  - **Personality**: Aggressive, cautious, idealistic, pragmatic
  - **Personal agenda**: Advancement, vengeance, ideology, loyalty
  - **Loyalty level**: How much they support player decisions
  - **Competency**: Bonus/malus to their domain's effectiveness
  - **Relationships**: Internal rivalries and alliances
- Council meetings:
  - Present current situation (military threat, economic crisis, succession dispute)
  - Each councillor suggests approach based on their agenda
  - Player decides final policy
  - Councillors approve or disapprove (affecting loyalty)
  - Decision has measurable impact on world state

**Judicial System**
- Player conducts public trials for:
  - Criminal accusations (murder, theft, treason)
  - Civil disputes (property claims, broken contracts)
  - Political crimes (heresy, sedition, adultery of nobility)
- Each trial involves:
  - **Accusation**: Formal charge with evidence
  - **Defense**: Counterargument and witness testimony
  - **Context**: Background of accused and accuser
  - **Precedent**: Previous relevant judgments by player
  - **Political Impact**: How verdict affects factions and reputation
- Possible verdicts:
  - **Innocent**: Free the accused (may anger accuser)
  - **Guilty**: Punish appropriately (execution, exile, fines, servitude)
  - **Conditional**: Complex verdicts (exile but keep family, fine but no execution)
  - **Compromise**: Split the difference (satisfy neither party but no extreme resentment)
- Consequences:
  - Innocent verdict: Risk bloodfeud with accuser if they're powerful
  - Harsh verdicts: Creates sympathy for accused among population
  - Lenient verdicts: Suggests favoritism, undermines justice system
  - Pattern of verdicts: Reputation as just, merciful, corrupt, or tyrannical

**Law System**
- Player can propose and enact laws:
  - **Labor laws**: Working conditions, slavery regulations
  - **Religious laws**: Tolerance or persecution of faiths
  - **Economic laws**: Trade regulations, tax rates, merchant rights
  - **Military laws**: Conscription, standing army size
  - **Civil laws**: Marriage, divorce, property rights
- Law effects:
  - Change taxation efficiency
  - Affect population happiness in regions
  - Create political opposition among affected groups
  - Require enforcement (cost and effort)
  - Can be repealed by future rulers
- Enforcement:
  - Laws without enforcement are ignored
  - Strict enforcement is costly but effective
  - Selective enforcement creates corruption/resentment
  - Lack of enforcement undermines authority

**Military Strategy**
- Player commands military forces with:
  - **Army composition**: Infantry, cavalry, archers, siege equipment
  - **Commander selection**: Different generals have different strategies and morale bonuses
  - **Supply management**: Logistics determine how long army can operate
  - **Morale**: Victories increase it, retreats decrease it
  - **Fatigue**: Armies weaken over time without rest
- Military operations:
  - Battles use tactical turn-based system
  - Strategic map shows army positions and enemy movements
  - Player chooses engagement, ambush, siege, or retreat
  - Losses are permanent and affect future strength
  - Victories can be achieved with different tactics (honor, cunning, overwhelming force)

---

### Pillar 4: Dynamic Political World

**Inspiration**: Mount & Blade: Bannerlord, Crusader Kings III, Europa Universalis

#### Objectives
- Create autonomous faction simulation with independent AI goals
- Implement diplomacy system with real bargaining power
- Build economic simulation affecting all regions
- Create organic rebellion and political upheaval mechanics

#### Core Mechanics

**Faction AI System**
- Each major faction:
  - **Goal hierarchy**: Primary goal (territorial expansion, wealth, religious dominance) and secondary goals
  - **Resource tracking**: Military strength, economic wealth, espionage capability, religious influence
  - **Diplomacy stance**: Friendly, neutral, hostile toward other factions
  - **Leadership**: Individual leaders with their own agendas
  - **Decision making**: AI evaluates options and chooses based on goal optimization
  - **Memory**: Remembers past betrayals and alliances
- Faction actions:
  - Recruit armies and position them
  - Make diplomatic proposals
  - Invest in infrastructure (temples, markets, walls)
  - Conduct espionage and sabotage
  - Negotiate marriages and alliances
  - Break agreements when beneficial

**Diplomatic System**
- Player can initiate diplomatic interactions:
  - **Trade agreements**: Exchange resources for mutual benefit
  - **Non-aggression pacts**: Agree to not attack each other (temporary)
  - **Alliances**: Agree to support each other militarily
  - **Vassalization**: Weaker faction submits to stronger (complex relationship)
  - **Marriage**: Diplomatic marriage between leaders to cement alliance
  - **Tribute**: Weaker faction pays stronger to avoid conflict
  - **Espionage**: Secret agreements, spying, sabotage offers
- Negotiation mechanics:
  - Each party has demands and acceptable terms
  - Prestige/power determines negotiating position
  - Faction leaders have personality (generous, aggressive, paranoid)
  - Breaking agreements has reputation consequences
  - Some agreements can be secretly sabotaged

**Economic Simulation**
- Regional economy affected by:
  - **Trade routes**: Active routes increase prosperity
  - **Agriculture**: Harvests vary by season and region
  - **Resource extraction**: Mining, logging, fishing
  - **Manufacturing**: Craftspeople producing goods
  - **Population**: More citizens = more consumption and labor
  - **Security**: Banditry and war reduce economic output
  - **Taxation**: Revenue collection with diminishing returns if too high
- Economic cascade:
  - Low prosperity → reduced tax revenue → weaker military → more vulnerable to attack
  - High prosperity → population growth → increased military recruitment → stronger faction
  - Unequal wealth distribution → population unrest → rebellion risk

**Rebellion & Uprising System**
- Population discontent builds from:
  - **Harsh judgments**: Executions, exiles, heavy fines
  - **High taxes**: Reduces quality of life
  - **Military failures**: Defeats reduce morale
  - **Oppressive laws**: Restrictions on freedom or rights
  - **Neglect**: Lack of investment in infrastructure
  - **Religious persecution**: Crackdown on popular faiths
- Discontent manifests as:
  - **Unrest**: Hidden discontent affecting NPC disposition
  - **Protests**: Open dissent in towns and cities
  - **Rebellion**: Armed uprising by population
  - **Succession crisis**: Questioning of legitimacy
  - **Civil war**: Factional breakdown into hostile groups
- Rebellion resolution:
  - **Suppression**: Military force (expensive, creates more discontent)
  - **Appeasement**: Reverse unpopular policies (loses previous benefits)
  - **Negotiation**: Compromise with rebel leadership (reduces their influence)
  - **Exile/execution**: Kill rebel leaders (prevents future trouble but hardens opposition)

**Espionage System**
- Player can engage in covert operations:
  - **Assassination**: Remove enemy leader or rival
  - **Sabotage**: Destroy infrastructure or supplies
  - **Recruitment**: Turn enemy agents to your side
  - **Propaganda**: Spread rumors affecting NPC opinions
  - **Theft**: Steal resources or secrets
  - **Blackmail**: Leverage discovered secrets for compliance
- Espionage mechanics:
  - Operations have success chance based on:
    - Agent skill level
    - Target security
    - Resources invested
    - Difficulty of objective
  - Failed operations can backfire (agent captured, alliance damaged)
  - Discovered operations create diplomatic incident
  - Some operations create permanent consequences

---

## Core Gameplay Loops

### Personal Relationship Loop

```
Discovery → Investment → Conflict → Resolution → Deepening
```

**Example Flow**:
1. **Discovery**: Meet companion, learn about their past
2. **Investment**: Spend time together, complete their personal quest
3. **Conflict**: Their goal conflicts with your political decision
4. **Resolution**: Choose to support them, compromise, or prioritize politics
5. **Deepening**: Relationship changes based on your choice

**Duration**: Spans entire game; can evolve into romance, deep friendship, or betrayal

---

### Political Consequence Loop

```
Decision → Implementation → Reaction → New Situation → New Decision
```

**Example Flow**:
1. **Decision**: Reduce taxes to increase popularity
2. **Implementation**: Treasury revenue drops 30%
3. **Reaction**: Military budget can't be fully funded; generals complain
4. **New Situation**: Enemy faction notices military weakness, threatens border
5. **New Decision**: Raise taxes back up (losing popularity), borrow money (debt), or negotiate peace

**Duration**: Multiple gameplay sessions; creates emerging narrative tension

---

### Systemic World Loop

```
Action → World State Change → NPC Reaction → New Opportunity/Conflict
```

**Example Flow**:
1. **Action**: Marry off companion to rival faction's leader
2. **World State Change**: Alliance between factions formed
3. **NPC Reaction**: 
   - Companion must balance loyalty between you and spouse
   - Third faction feels threatened by new alliance
   - Population gossips about the marriage's political implications
4. **New Opportunity/Conflict**:
   - New alliance enables military cooperation against common enemy
   - BUT romantic rival of your companion harbors resentment
   - Spouse's faction begins making demands as price of alliance
   - Potential for marriage to dissolve if conflicts arise

**Duration**: Months of game time; resolves into new equilibrium or explodes into crisis

---

## System Architecture

### Core Systems Overview

```
Game State Manager
├── Persistent World State (flags, timeline, location status)
├── Character System
│   ├── Relationship Manager
│   ├── Companion Arc Tracker
│   └── NPC Personality Engine
├── Political System
│   ├── Faction Manager
│   ├── Diplomacy Engine
│   └── Council System
├── Economic System
│   ├── Regional Prosperity Tracker
│   ├── Trade Route Manager
│   └── Player Wealth Manager
├── Judicial System
│   ├── Trial Manager
│   ├── Verdict Consequence Engine
│   └── Legal Framework Database
├── Military System
│   ├── Army Manager
│   ├── Battle Resolver
│   └── Strategic Map
└── Consequence Engine (connects all systems)
```

### Persistent State Requirements

The game must track:
- **Character State**: 200+ NPCs with relationship data, personality, goals, location, employment
- **World State**: 50+ major locations with prosperity, population, infrastructure, faction control
- **Political State**: 8-12 factions with military strength, treasury, diplomacy stance, territory
- **Event Flags**: 1000+ flags representing past decisions and their consequences
- **Timeline**: Game events ordered chronologically (used for retrospective consequence calculation)
- **Economic State**: Regional prices, trade routes, supply/demand, player income sources

### Save Game Structure

```json
{
  "metadata": {
    "version": "1.0",
    "playthrough_id": "uuid",
    "current_date": "game_time",
    "playtime_hours": 0.0
  },
  "player_state": {
    "current_location": "location_id",
    "inventory": [],
    "stats": {},
    "relationships": {},
    "reputation": {},
    "wealth": 0,
    "properties": []
  },
  "world_state": {
    "npc_data": {},
    "location_data": {},
    "faction_data": {},
    "event_flags": {},
    "timeline": []
  },
  "game_time": 0
}
```

---

## Consequence System

### The Integrated Impact Model

Every player action is evaluated across three dimensions:

#### Personal Dimension
- How does this affect the player's relationships?
- What are the emotional/roleplay consequences?
- Does it align with the player's character identity?

#### Political Dimension
- How do factions react?
- What shifts in power dynamics occur?
- Are new alliances or rivalries created?

#### Systemic Dimension
- How does this alter world state?
- What cascading effects occur?
- Are new opportunities or obstacles created?

### Consequence Cascade Example

**Scenario**: Player executes a popular nobleman for treason

**Immediate Consequences**:
- Political: His family now enemies, his allies question loyalties
- Systemic: Removes a political threat, but instills fear
- Personal: Companions react based on their values (some approve, some horrified)

**Secondary Consequences** (Days later):
- His widow hires assassins; assassination attempts begin
- His faction begins covert support for anti-player rebels
- Public opinion shifts based on perceived fairness vs. cruelty
- Military unit loyal to executed noble becomes unreliable

**Tertiary Consequences** (Weeks later):
- Rebels grow stronger with faction support
- If uncontrolled: rebellion spreads to neighboring regions
- Other nobles become nervous, potentially betraying player
- Economy weakens as people fear instability

**Quaternary Consequences** (Months later):
- Possible civil war if rebellion becomes large enough
- Entire political landscape shifts based on how rebellion was handled
- History records player as tyrant or just ruler based on context

### Consequence Tracking System

The game maintains a **consequence ledger**:

```
Action ID: EXEC_NOBLE_001
Action: Execute Lord Harren for treason
Date: Day 47
Affected Parties: 
  - Family: Harren House (now hostile)
  - Allies: 3 other minor lords (loyalty -15 each)
  - Population: General distrust (+20% unrest)
  - Military: 200 troops defect to rebels

Follow-up Events Triggered:
  - Assassination attempts: 2-4 per month
  - Rebellion: Harren House raises army
  - Trial precedent: Sets expectation for noble trials
  - Economic: Region affected by military activity

Current Status: CASCADING
Timeline: Day 47 - present (ongoing)
```

---

## Morality Framework

### Four-Axis Ethical System

Instead of good/evil alignment, players navigate four independent axes:

#### 1. Justice ↔ Pragmatism
- **Justice side**: Rule of law, fairness, precedent-based decisions
- **Pragmatism side**: Practical outcomes, "whatever works," results-focused
- **Example decision**: Innocent peasant could serve as a scapegoat for a crime. Pure justice says free him; pragmatism says sacrifice one to prevent riots.

#### 2. Honor ↔ Manipulation
- **Honor side**: Direct communication, integrity, keeping promises
- **Manipulation side**: Cunning, deception, using people for advantage
- **Example decision**: Ally needs help. Honor is to directly support them; manipulation is to make them owe you by "rescuing" them from a problem you secretly created.

#### 3. Freedom ↔ Control
- **Freedom side**: Individual liberty, minimal restrictions, personal choice
- **Control side**: Order through authority, collective good over individual wants
- **Example decision**: Implement conscription (control) vs. volunteer army (freedom). First is cheaper but resented; second costs more but is popular.

#### 4. Idealism ↔ Realpolitik
- **Idealism side**: Beliefs matter; pursue noble goals even if difficult
- **Realpolitik side**: Power matters; ideology is secondary to strategic advantage
- **Example decision**: Support distant refugees (idealism) vs. focus on immediate security (realpolitik).

### NPC Response to Morality Axes

Each NPC has their own axis alignment:

```
NPC: Ser Gareth (Knight Commander)
Justice: 8/10 (believes strongly in law)
Honor: 9/10 (personal integrity is core)
Freedom: 4/10 (believes in hierarchy and order)
Realpolitik: 3/10 (idealistic, doesn't think pragmatically)
```

When player makes decisions:
- **Aligned decisions** (player's choice matches NPC's values): Relationship improves, NPC more loyal
- **Opposed decisions**: Relationship strains, but not necessarily broken
- **Extreme oppositions** (constant conflicting values): NPC may leave service or betray player

### Player Axis Tracking

The player's axis position evolves:
- Starting position: Balanced (5 on each axis)
- Each decision shifts position along relevant axis
- Major decisions have larger shifts
- Consistent decisions strengthen position (easier to maintain that position)
- Drastic reversal (pragmatist becoming idealist) is possible but impactful

### Axis Display & Feedback

Player sees:

```
        Idealism
            |
    (Your position: 6)
            |
Justice ----●---- Pragmatism
    (8)     |     (2)
            |
         Control
```

This is updated after major decisions, showing growth in each direction.

---

## Romance & Relationship System

### Philosophy

Romances are not "approval meters." They are complex relationships with genuine conflict, passion, and consequence.

### Relationship Progression

#### Stage 1: Acquaintance
- Met the person; basic impression formed
- Early dialogue options open
- Can invite to events (tavern, hunts)
- Relationship can stall or deepen

#### Stage 2: Companionship
- Spend significant time together
- Learn about their background
- Help with personal quests
- Romantic potential becomes visible (if applicable)
- Can begin Romance sub-path or remain friends

#### Stage 3: Romance (if applicable)
- Physical and emotional intimacy begins
- Conflicts may emerge (jealousy, differing goals, lifestyle clashes)
- Options: Continue, Cool down, or Break up
- If broken up: Can become resentful rivals or wistful exes

#### Stage 4: Commitment
- Marriage proposal (or equivalent commitment)
- Merges economic interests
- Can create children
- Introduces spouse into your political life
- New conflicts: Family interests vs. political interests

#### Stage 5: Legacy
- Children are born
- Spouse becomes part of ruling structure
- Potential for spouse to resent political decisions affecting family
- Possible divorce if irreconcilable differences

### Romance Mechanics

**Romantic Interest Factors**:
- **Attraction**: Physical appeal (gameplay abstraction, not explicit)
- **Personality Compatibility**: Shared values on morality axes
- **Shared Experiences**: Adventures, crises overcome together
- **Vulnerability**: Emotional openness and trust
- **Absence**: Distance and time apart create emotional effect
- **Jealousy**: Romantic rival interest or player's other relationships

**Romance Complications**:
- **Ideological Conflict**: Partner strongly disagrees with political decision
- **Career Ambition**: Partner's personal goals conflict with yours
- **Arranged Marriage**: Spouse was diplomatic arrangement, now wants genuine love
- **Betrayal**: Discovery of infidelity or deceit
- **Family Pressure**: Spouse's family has demands on you
- **Succession Issues**: Debate over children's future
- **Power Imbalance**: Being in power creates friction (subject/ruler dynamic)

### Marriage & Dynasty

**Marriage Mechanics**:
- Marriage creates alliance with spouse's faction
- Spouse moves into your residence
- Spouse can become pregnant
- Spouse might join your council (if politically ambitious)
- Spouse has their own agenda and personality

**Divorce Mechanics**:
- Can be initiated by player or spouse
- Requires political negotiation if spouse is important
- Custody of children becomes issue
- Alliance may be broken (return to hostile relations)
- Alimony or financial settlement may be required
- Social scandal affects reputation

**Children & Heirs**:
- Each child has:
  - Genetic traits inherited from parents
  - Personality developed through upbringing
  - Education pathway (military, scholarly, diplomatic, religious)
  - Marriage potential (can arrange advantageous marriages)
- Player can:
  - Spend time with children (affects personality)
  - Choose education (affects skills)
  - Arrange marriages (affects alliances)
  - Show favoritism (affects inheritance)
- At player death (if applicable), heir takes over
- If multiple heirs, succession can be disputed (civil war possible)

---

## Political Simulation

### Council System

The player leads a council of 5-7 major NPCs:

| Council Role | Responsibility | Type |
|-------------|----------------|------|
| Master of War | Military strategy, defense | Tactical |
| Master of Coin | Economy, taxes, trade | Economic |
| Spymaster | Intelligence, espionage, secrets | Information |
| High Judged | Justice, laws, trials | Judicial |
| Master of Whispers | Diplomacy, rumors, intrigue | Political |
| (Religious figure if applicable) | Faith, morality, legitimacy | Ideological |

**Council Meeting Mechanics**:
- Triggered by major events or player summons
- Current situation presented
- Each councillor recommends approach (based on their role and personality)
- Player chooses approach
- Councillors vote approve/disapprove
- Decision is implemented with consequences

**Councillor Loyalty**:
- Increases when player follows their recommendation
- Decreases when player overrules them repeatedly
- Low loyalty: Councillor may:
  - Resign and become opposition
  - Actively work against player behind scenes
  - Leak information to enemies
  - Refuse orders in critical moment
- High loyalty: Councillor assists beyond stated duties, provides bonuses

### Faction Dynamics

**Playable vs. Non-Playable Factions**:
- Player controls one primary faction (their kingdom/region)
- 5-8 neighboring/rival factions operate with independent AI
- Each faction has:
  - Leader with personality and ambitions
  - Territory and major cities
  - Military strength
  - Economic wealth
  - Relationship network with other factions

**Diplomatic Relations**:

Relations range from -100 (implacable enemies) to +100 (perfect allies):

- **-100 to -75**: Actively at war, assassination attempts, sabotage
- **-75 to -50**: Hostile, looking for excuse to attack, embargo
- **-50 to 0**: Mistrustful, cold relations, some trade
- **0 to 50**: Neutral, potential for cooperation
- **50 to 75**: Friendly, willing to trade and share intelligence
- **75 to 100**: Allied, military cooperation, marriage alliances

**Faction Goals**:
- Each faction has primary goal (territorial expansion, religious dominance, trade control, etc.)
- Secondary objectives support primary goal
- Goals can shift based on circumstances
- Player actions blocking goals create resentment

### Trade & Economics

**Trade Route System**:
- Routes connect major cities
- Can be secured (protected from bandits) or risky
- Routes affected by faction relations (hostile faction may block routes)
- Each route generates income if secured

**Taxation Model**:
- Player sets tax rate (0-25% of regional income)
- Higher taxes generate more revenue but reduce population happiness
- Tax evasion increases with resentment
- Effective tax income = Base × (1 - evasion %) × (quality of collection)
- Overextension possible (pushing taxes too high causes immediate unrest)

**Economic Crisis**:
- Triggered by excessive taxation, military defeats, or natural disasters
- Symptoms: Reduced trade, population emigration, reduced military capability
- Recovery requires investment and time

---

## World Design

### Region Structure

The world consists of 3-5 major regions, each with:

**Geographic Features**:
- Capital city (large, defensible, economic hub)
- 3-8 towns and villages
- Fortifications and strongholds
- Terrain (mountains, forests, plains, coast)

**Regional Economy**:
- Primary industry (mining, farming, fishing, trade)
- Resource production rates
- Prosperity level (affects all metrics)
- Population size

**Regional Politics**:
- Major local factions
- Historical rivalries
- Cultural identity
- Religious traditions

### Dynamic World Changes

**Visual Evolution**:
- Cities grow/shrink based on prosperity
- Fortifications damaged by war become visible
- Trade posts appear/disappear based on routes
- Temples built/destroyed based on religious policy

**Population Migration**:
- People flee from dangerous/impoverished regions
- Move toward prosperity and safety
- Migration affects military recruitment potential

**Natural Events**:
- Seasonal harvests affect economy
- Natural disasters (drought, plague) create crises
- Weather affects military operations

---

## End-Game Systems

### Multiple Endings Concept

Rather than a single "ending," the game continues with:

**Victory Conditions** (if player desires narrative closure):
- Defeat all enemies militarily
- Achieve economic dominance
- Achieve religious dominance
- Achieve political dominance
- Fulfill a personal goal (romance, family, legacy)

**Succession System**:
- If player character dies (age, illness, combat), heir takes over
- New ruler inherits:
  - Titles and position
  - Relationships (modified by how predecessor treated people)
  - Economic resources
  - Political enemies and allies
  - Personal traits from genetics and upbringing

**Legacy Tracking**:
- Game records:
  - Major decisions made
  - History written about player character
  - Heir's relationship to predecessor's choices
  - Long-term impacts of decisions

**New Game+**:
- Play with knowledge of consequence system
- Attempt different approach
- See how different choices lead to different worlds

---

## Technical Considerations

### Engine Requirements

**Language & Framework**:
- Recommendation: C++ (Unreal Engine 5, custom engine, or similar)
- Alternative: C# (Unity) if optimized carefully
- Requirement: Support for:
  - Complex state management
  - Background AI simulation
  - Dynamic dialogue system
  - Turn-based tactical combat
  - Large world simulation

**Performance Targets**:
- World simulation runs in background without impacting framerate
- NPC AI updates on schedule (not every frame)
- Dialogue system has <100ms response time
- Combat system: 60 FPS during tactical encounters
- Open world: 30-60 FPS depending on density

### Data Structures

**NPC Data**:
```cpp
struct NPC {
    string id;
    string name;
    // Personality
    int morality_axes[4]; // -10 to +10 on each axis
    string personality_traits[];
    // Relationships
    map<string, RelationshipData> relationships; // To other NPCs and player
    // Goals and status
    Goal primary_goal;
    Goal secondary_goals[];
    Location current_location;
    Employment current_employment;
    // Flags and history
    set<string> event_flags;
    vector<Interaction> interaction_history;
};

struct RelationshipData {
    float rapport;     // -100 to +100
    float trust;       // -100 to +100
    float alignment;   // Shared values on morality axes
    float romantic_potential; // 0 if N/A, 0-100 if possible
    bool is_married;
    int years_known;
    set<string> conflict_flags;
};
```

**World State**:
```cpp
struct GameState {
    // Timeline
    int current_day;
    
    // Characters
    map<string, NPC> npcs;
    PlayerCharacter player;
    
    // Factions
    map<string, Faction> factions;
    
    // Locations
    map<string, Location> locations;
    
    // Event flags (thousands of them)
    set<string> global_flags;
    
    // Persistent consequence tracking
    vector<ConsequenceEvent> consequence_log;
};
```

### Dialogue System

**Requirements**:
- Support branching dialogue with hundreds of variations
- Track dialogue history and reference past conversations
- Gate dialogue options based on:
  - Relationship status
  - Morality alignment
  - Event flags
  - Character knowledge
  - NPC personality
- Generate dynamic responses based on:
  - Current world state
  - NPC emotional state
  - Conversation history
  - Hidden secrets

**Implementation Approach**:
- Structured dialogue trees with conditional logic
- Scripting language for dialogue branching (Lua or custom)
- Dynamic text generation for situational references
- Voiceover support (optional; text-based initially)

### Save System

**Requirements**:
- Complete world state serialization
- Fast save/load (<5 seconds)
- Multiple save slots
- Autosave system (backup every 15 minutes)
- Save compatibility (allow older saves to load with migration)

**Size Considerations**:
- Estimate: 5-20 MB per save file
- With character/location/faction data, dialogue history, flags

### Testing & QA

**Critical Testing Areas**:
- Consequence chain verification (decisions have intended effects)
- Dialogue gating (options unavailable when they should be)
- Relationship state consistency
- Economic balance (inflation, deflation prevention)
- AI faction behavior (goals being pursued, not stuck)
- Save/load integrity
- Performance under heavy world state

---

## Development Roadmap

### Phase 1: Foundation (Months 1-6)
- [ ] Core game loop and architecture
- [ ] Basic character and relationship system
- [ ] Simple dialogue system
- [ ] Tutorial area with 2-3 NPCs

### Phase 2: Systems (Months 7-12)
- [ ] Full morality axis system
- [ ] Political simulation (factions, diplomacy)
- [ ] Council system
- [ ] Economic simulation basics
- [ ] Combat system (if not prototyped in Phase 1)

### Phase 3: Content (Months 13-18)
- [ ] Full cast of 50+ NPCs with companion arcs
- [ ] 3-5 major regions fully realized
- [ ] Hundreds of dialogue variations
- [ ] Complete faction interaction systems
- [ ] Romance systems fully implemented

### Phase 4: Integration & Polish (Months 19-24)
- [ ] Consequence system fully integrated
- [ ] End-game succession system
- [ ] Balance pass on all systems
- [ ] Performance optimization
- [ ] Extensive playtesting and iteration

---

## Glossary of Terms

| Term | Definition |
|------|-----------|
| **Morality Axes** | Four independent ethical dimensions (Justice, Honor, Freedom, Idealism) that define character values |
| **Relationship Depth** | Complex relationship tracking beyond simple approval ratings |
| **Consequence Cascade** | When a single decision triggers multiple secondary and tertiary effects |
| **Event Flags** | Boolean values tracking whether specific events have occurred |
| **Faction AI** | Autonomous decision-making by non-player factions pursuing goals |
| **Companion Arc** | Personal narrative storyline for major character companion |
| **Council System** | Group of advisors with different specialties who offer counsel and execute orders |
| **Diplomatic Marriage** | Marriage between characters for political alliance rather than romance |
| **Succession** | Process of transferring power to heir when current ruler dies or abdicates |
| **Emergent Narrative** | Story created through systemic interaction rather than predetermined script |

---

## Conclusion

**The Eternal Throne** aims to be a game where systemic depth creates narrative richness. Rather than choosing between simulation and story, it integrates them: story emerges from the simulation, and the simulation is constrained to enable meaningful story.

This requires:
1. **Sophisticated systems** that interact intelligently
2. **Massive content** to support emergent narratives
3. **Clear design** to prevent systems from becoming chaotic
4. **Careful balance** between player agency and meaningful challenge
5. **Iterative playtesting** to ensure systems feel fair and rewarding

The result should be a game that tells a different story with each playthrough, where players feel the weight of their choices reverberating across decades of game time.

---

**Document Version**: 1.0  
**Last Updated**: February 2026  
**Status**: Design Complete - Ready for Development Planning
