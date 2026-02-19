# Dialogue and Character Addition Guide

This guide provides detailed instructions on how to add new dialogue and new characters to the GameDevSolitaire project.

## Table of Contents
1. [Project Structure Overview](#project-structure-overview)
2. [Adding New Characters](#adding-new-characters)
3. [Adding New Dialogue](#adding-new-dialogue)
4. [Advanced Features](#advanced-features)

---

## Project Structure Overview

This project is built with Unity Engine and uses an XNode-based dialogue system. The main file structure is:

```
ProjectUnity/Client/
├── Assets/
│   ├── Scripts/
│   │   ├── Dialog/                    # Core dialogue system scripts
│   │   │   ├── DialogGraph.cs         # Dialogue graph
│   │   │   └── DialogNode.cs          # Dialogue node
│   │   ├── Game/Character/             # Character system
│   │   │   ├── Character.cs           # Character class
│   │   │   ├── CharacterCA.cs         # Character configuration data
│   │   │   └── CharacterFactory.cs    # Character factory
│   │   └── Panel/Dialog/
│   │       └── DialogPanel.cs         # Dialogue panel UI
│   ├── Resources/
│   │   ├── Dialog/                    # Dialogue asset files
│   │   │   ├── 游戏开始.asset
│   │   │   ├── 第2日起床.asset
│   │   │   └── ...
│   │   └── Character/                 # Character prefabs
│   │       ├── C1.prefab
│   │       ├── C2.prefab
│   │       └── C3.prefab
│   └── StreamingAssets/
│       └── Data/
│           ├── character.csv          # Character data table
│           ├── daily.csv              # Daily dialogue data table
│           ├── map.csv                # Map scene data table
│           └── asset.csv              # Asset/item data table
```

### Key Concepts

- **DialogGraph**: Dialogue graph containing multiple dialogue nodes, defining the complete dialogue flow
- **DialogNode**: Single dialogue node containing dialogue text, choices, rewards, conditions, etc.
- **Character**: NPC characters in the game
- **CharacterCA**: Character configuration data (Configuration Asset)
- **CSV Data Tables**: Data tables storing basic character information

---

## Adding New Characters

### Step 1: Prepare Character 3D Model or Prefab

1. Create a 3D model or 2D sprite for the character in Unity
2. Add necessary components to the character (e.g., animator, collider)
3. Save the character as a Prefab

**File Location**: `Assets/Resources/Character/`

**Naming Convention**: Use `C` + number format, e.g., `C4.prefab`, `C5.prefab`

### Step 2: Add Character Information to CSV Data Table

Open file: `ProjectUnity/Client/Assets/StreamingAssets/Data/character.csv`

Add a new row following this format:

```csv
ID,名称,地址
cid,name,path
1200001,少女,Character/C1
1200002,市长凯恩,Character/C2
1200003,牧场老板李北镇,Character/C3
1200004,New Character Name,Character/C4  ← Add new character
```

**Field Descriptions**:
- **ID** (cid): Unique character identifier, recommended format: 120xxxx
- **名称** (name): Character name displayed in the game
- **地址** (path): Character prefab path in Resources folder (without .prefab extension)

### Step 3: Place Character in Scene

If you need to place the character in a specific scene, there are two methods:

**Method A: Place Directly in Scene**
1. Open target scene (e.g., `Assets/Scenes/Map/Shop.unity`)
2. Drag the character prefab into the scene
3. Adjust position and rotation
4. Add click interaction functionality to the character (add Collider and script)

**Method B: Dynamically Generate via Script**
Create characters using CharacterFactory in the scene manager:

```csharp
CharacterFactory cf = CBus.Instance.GetFactory(FactoryName.CharacterFactory) as CharacterFactory;
Character character = cf.Create(1200004) as Character; // Use the new character's ID
```

### Step 4: Create Character Dialogue Content

After creating the character, add dialogue content (see "Adding New Dialogue" section).

---

## Adding New Dialogue

### Method 1: Create Dialogue Graph Using XNode Editor (Recommended)

#### Step 1: Create DialogGraph Asset

1. In Unity Project window, right-click the `Assets/Resources/Dialog/` folder
2. Select `Create > DialogSystem > DialogGraph`
3. Name the dialogue graph, e.g., `New Character Dialogue` or `Day 4 Morning`

**Naming Convention**:
- Character dialogue: `{CharacterName}{CharacterID}/{CharacterID}_{SceneName}_{Day}`
  - The format directly concatenates character name with ID (no separator)
  - This is a **human-readable convention** - the system looks up files by the full path string
  - Example: `少女1200001/1200001_Shop_1` (少女 is character name, 1200001 is ID)
  - For English: `Blacksmith1200004/1200004_Shop_1`
  - **Note**: Character IDs follow the format 120xxxx (7 digits starting with 120)
- Daily story: `第{X}日{TimeOfDay}` (Chinese format)
  - Example: `第4日起床`, `第5日晚上`
  - Or use English: `Day4Morning`, `Day5Night`

#### Step 2: Edit Dialogue Using XNode Editor

1. Double-click the created DialogGraph asset to open the XNode editor window
2. Right-click on empty space, select `Add Node > Dialog Node`
3. Configure parameters for each dialogue node

#### Step 3: Configure DialogNode Parameters

Each DialogNode contains the following configurable fields:

##### Basic Fields
- **input**: Input port (connects to previous node)
- **speakerid**: Speaker ID (corresponds to character ID in character.csv, 0 = narrator)
- **ani**: Animation name (optional, triggers character animation)
- **dialogText**: List of dialogue text (can add multiple, displayed in order)
  - Click `+` to add new dialogue text
  - Supports multiline text (displayed using TextArea)

##### Rewards and Costs
- **rewards**: List of rewards obtained after dialogue completion
  - Format: `Pair<int, int>` (Asset ID, Quantity)
  - Example: Receive 10 coins
- **cost**: Resources required for the dialogue
  - Format: `Pair<int, int>` (Asset ID, Quantity)

##### Choice System
- **choices**: List of dialogue choices available to the player
  - Each choice contains:
    - **choiceText**: Text displayed for the choice
    - **conditions**: List of conditions for displaying the choice
      - **id**: Attribute ID
      - **comparisonType**: Comparison type enum value (from `Condition.ComparisonType` enum in DialogNode.cs)
        - `GreaterThan` (>)
        - `LessThan` (<)
        - `EqualTo` (==)
        - `GreaterOrEqual` (>=)
        - `LessOrEqual` (<=)
      - **value**: Target value

##### Scene Control
- **scene**: Scene name to jump to after dialogue ends (optional)
- **prefab**: Prefab name to load in the scene (optional)
- **endingDescription**: Ending description text (if this is an ending node)
- **eventid**: Event ID to trigger (optional)

##### Output Ports
- **nextNode**: Dynamic output ports, automatically generated based on number of choices
  - No choices: Only one default output port
  - With choices: One output port per choice

#### Step 4: Connect Dialogue Nodes

1. Drag from a node's output port to another node's input port
2. Create dialogue flow graph
3. Ensure there is a clear start node (graph.nodes[0])

#### Example Dialogue Graph Structure

```
[Start Node] 
    ↓
[Character Greets]
    ↓
[Player Choice] ─→ [Option A] → [Result A]
    ├─→ [Option B] → [Result B]
    └─→ [Option C] → [Result C]
```

### Method 2: Call Dialogue via Code

In code, you can call dialogue through DialogManager:

```csharp
// Method 1: Call dialogue by name
DialogManager dm = CBus.Instance.GetManager(ManagerName.DialogManager) as DialogManager;
dm.ShowDialog("游戏开始");

// Method 2: Trigger when clicking character
// When player clicks character, system automatically searches for corresponding dialogue
// Dialogue naming format: {CharacterName}{CharacterID}/{CharacterID}_{SceneName}_{Day}
// Example: 少女1200001/1200001_Shop_2
```

### Step 5: Configure Dialogue in Daily Data (Optional)

If the dialogue is daily fixed story content, add it to `daily.csv`:

Open file: `ProjectUnity/Client/Assets/StreamingAssets/Data/daily.csv`

```csv
ID,名称,聊天内容,强制跳转
cid,name,dialog,scene
1400001,第1日清晨,第1日起床,
1400002,第2日清晨,第2日起床,
1400003,第3日清晨,第3日起床,
1400004,第4日清晨,第4日起床,  ← Add new daily dialogue
```

---

## Advanced Features

### Conditional Choice System

Dialogue choices can have display conditions - players will only see choices when conditions are met.

**Example**: Only show special option when favorability >= 50

1. Add choice to DialogNode's choices
2. Add Condition to choice:
   - id: Favorability attribute ID
   - comparisonType: GreaterOrEqual
   - value: 50

### Reward and Cost System

Dialogue can give rewards or consume resources from the player.

**Configuration Method**:
1. Add reward items to DialogNode's `rewards` field
   - This is a list of `Pair<int, int>` where:
     - First value (k): Asset ID (refer to Assets/StreamingAssets/Data/asset.csv)
     - Second value (v): Quantity
2. Add cost items to DialogNode's `cost` field (same format)

**Note**: Asset IDs can be found in the `asset.csv` file which defines all in-game items, resources, and currencies.

**Effects**:
- Automatically processes rewards/costs when dialogue is displayed
- Shows notification message on screen
- Updates player inventory data

### Scene Transition

Dialogue can automatically transition to a new scene when it ends.

**Configuration Method**:
1. Enter target scene name in DialogNode's `scene` field
2. (Optional) Enter prefab name to load in `prefab` field

**Example**:
```
scene: Shop
prefab: LobbyPanel
```

### Event Triggering

Trigger game events when dialogue ends.

**Configuration Method**:
1. Enter event ID in DialogNode's `eventid` field
2. Event will be handled by EventManager

### Dialogue Typewriter Effect

The system enables typewriter effect by default (displays dialogue text character by character).

**Adjust Speed**:
- Modify `typingSpeed` variable in DialogPanel.cs
- Default value: 0.05 seconds/character

### Dialogue History

The system automatically saves completed dialogues.

**Mechanism**:
- Saves using PlayerPrefs
- Key format: Dialogue graph name
- Value: endingDescription content
- Shows saved ending description when triggered again

---

## Complete Example: Adding New Character and Configuring Dialogue

### Scenario: Add a new NPC named "Blacksmith"

#### 1. Create Character Prefab
- Create blacksmith 3D model in Unity
- Save as `Assets/Resources/Character/C4.prefab`

#### 2. Add Character Data
Add to `character.csv`:
```csv
1200004,Blacksmith,Character/C4
```

#### 3. Create Dialogue Graph
- Create `Assets/Resources/Dialog/Blacksmith1200004/1200004_Shop_1.asset`
- Open XNode editor

#### 4. Configure Dialogue Nodes

**Node 1: Greeting**
```
speakerid: 1200004
dialogText: ["Hello, welcome to the blacksmith!"]
choices: [Continue]
```

**Node 2: Choose Topic**
```
speakerid: 1200004
dialogText: ["What can I help you with?"]
choices: 
  - "I want to buy a weapon" → Node 3
  - "I want to upgrade equipment" → Node 4
  - "Just passing through" → Node 5
```

**Node 3: Buy Weapon**
```
speakerid: 1200004
dialogText: ["This sword suits you well, only 100 coins."]
cost: [(Coin ID, 100)]
rewards: [(Sword ID, 1)]
choices: []  # Dialogue ends
endingDescription: "You purchased a new sword"
```

#### 5. Connect Nodes and Save

#### 6. Place Character in Scene
- Open Shop scene
- Drag C4 prefab into scene
- Adjust position

#### 7. Test
- Run game
- Click blacksmith character
- Verify dialogue displays correctly

---

## Important Notes

1. **ID Conventions**:
   - Character ID: 120xxxx
   - Daily data ID: 140xxxx
   - Asset ID: Refer to asset.csv

2. **File Naming**:
   - Dialogue asset files can use Chinese or English names
   - For character dialogues: Must follow the naming format `{CharacterName}{CharacterID}/{CharacterID}_{SceneName}_{Day}` to be recognized by system
   - The system looks up files by this exact path in Resources/Dialog folder

3. **Dialogue Flow**:
   - Ensure dialogue graph has clear ending node
   - Nodes without choices will automatically close dialogue panel

4. **Resource Paths**:
   - Paths in Resources folder don't need file extensions
   - Use forward slash `/` as path separator

5. **Testing Recommendations**:
   - Test immediately after adding new content
   - Check if dialogue triggers correctly
   - Verify reward and condition systems work properly

6. **Version Control**:
   - Pull latest branch before modifying CSV files
   - Avoid overwriting others' changes
   - Commit and merge changes promptly

---

## Common Issues

### Q: Dialogue won't trigger?
A: Check the following:
1. Is the dialogue asset file in Resources/Dialog folder?
2. Does the file name follow the required format?
3. Is the DialogGraph correctly configured with nodes?
4. Does the character ID exist in character.csv?

### Q: Character model doesn't show?
A: Check:
1. Is the prefab path correct?
2. Does the speakerid correspond to the correct character ID?
3. Is the character Prefab in Resources/Character folder?

### Q: Dialogue choices don't show?
A: Check:
1. Are choices correctly configured?
2. Is the condition system blocking the choice display?
3. Is UpdateDynamicPorts being called correctly?

### Q: How to debug the dialogue system?
A: 
1. Add Debug.Log statements in DialogPanel.cs
2. Check Unity Console for error messages
3. Use XNode editor to check node connections

---

## Quick Reference File Table

| Function | File Path |
|----------|-----------|
| Character data configuration | `Assets/StreamingAssets/Data/character.csv` |
| Asset/item data configuration | `Assets/StreamingAssets/Data/asset.csv` |
| Dialogue assets | `Assets/Resources/Dialog/` |
| Character prefabs | `Assets/Resources/Character/` |
| Dialogue system scripts | `Assets/Scripts/Dialog/` |
| Character system scripts | `Assets/Scripts/Game/Character/` |
| Dialogue UI panel | `Assets/Scripts/Panel/Dialog/DialogPanel.cs` |
| Daily story configuration | `Assets/StreamingAssets/Data/daily.csv` |
| Map scene configuration | `Assets/StreamingAssets/Data/map.csv` |

---

## Conclusion

This guide covers all the basic steps for adding new dialogues and characters to the project. If you have any questions, please refer to existing dialogue and character implementations in the project, or ask in the developer community.

Happy developing! 🎮
