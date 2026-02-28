# How to Add New Scenes or Maps

This document provides detailed instructions on how to add new scenes or maps to the game.

## Overview

The game is developed using Unity Engine and follows a data-driven design pattern. Scene and map configuration data is stored in CSV files, read by `MapFactory` to create `MapCA` objects, and then loaded by `SceneLoadManager` to display scenes and related prefabs.

## Steps to Add a New Scene

### Step 1: Create Scene File in Unity

1. Open Unity Editor
2. From Unity menu: `File` -> `New Scene`
3. Design your scene content (add objects, lighting, cameras, etc.)
4. Save the scene to `Assets/Scenes/` or `Assets/Scenes/Map/` directory
   - Example: `Assets/Scenes/Map/YourSceneName.unity`
5. Add the scene to Build Settings:
   - Open `File` -> `Build Settings`
   - Drag the new scene into "Scenes In Build" list

### Step 2: Create UI Panel Prefab (Optional)

If your map requires a specific UI interface:

1. Create a UI prefab in the `Assets/Resources/` directory
2. Prefabs typically contain Canvas, UI elements, etc.
3. Reference existing prefabs like `StorePanel`
4. Remember the prefab path (relative to Resources folder), e.g., `StorePanel`

### Step 3: Add Configuration to map.csv

Open file: `ProjectUnity/Client/Assets/StreamingAssets/Data/map.csv`

#### CSV Format

```csv
ID,Name,Unlock Day,Open Time,Scene Name,Prefab,Icon,Camera Path,Default Position,Default Animation,Controllable
cid,name,unlockday,opentime,scene,prefab,icon,campath,ptrans,pani,ctrl
```

#### Field Descriptions

| Field | Description | Example |
|-------|-------------|---------|
| **ID (cid)** | Unique map identifier, recommend 180xxxx format | `1800010` |
| **Name (name)** | Display name of the map | `Shop`, `Forest`, `Castle` |
| **Unlock Day (unlockday)** | Day when map becomes available | `1` (Day 1), `3` (Day 3) |
| **Open Time (opentime)** | Hours when map is accessible, separated by pipe | `1` pipe `2` pipe `3`...`24` (All day)<br>`8` pipe `9`...`20` (8AM to 8PM) |
| **Scene Name (scene)** | Unity scene file name (without .unity extension) | `Shop`, `Game`, `Forest` |
| **Prefab (prefab)** | UI prefab path (relative to Resources), separated by pipe for different game days | `StorePanel` pipe `StorePanel` (2 same prefabs)<br>`DayPanel` pipe `NightPanel` (Different prefabs per day) |
| **Icon (icon)** | Map icon path | `UI/地图_会见室` |
| **Camera Path (campath)** | Path to camera object in scene | `Base/Camera_A-Block_01` |
| **Default Position (ptrans)** | Player spawn point marker name | `Point`, `StartPoint` |
| **Default Animation (pani)** | Player default animation | `Talking_03`, `Idle` |
| **Controllable (ctrl)** | Allow player movement control<br>0=Non-controllable, 1=Controllable | `0`, `1` |

#### Example: Adding a New Map

Add a new line to map.csv:

```csv
1800010,Mystic Forest,2,6|7|8|9|10|11|12|13|14|15|16|17|18,Forest,ForestPanel|ForestPanel,UI/Map_Forest,Base/Camera_Forest,StartPoint,Idle,1
```

This example creates a map called "Mystic Forest":
- ID: 1800010
- Unlocks on Day 2
- Open from 6AM to 6PM daily
- Uses Forest scene
- Uses ForestPanel prefab
- Player can control character movement

### Step 4: Understanding Prefab Arrays

The `prefab` field uses `|` to separate multiple prefabs corresponding to different game days.

Code logic (in SceneLoadManager.cs):
```csharp
int day = Mathf.Clamp(gm.day - 1, 0, mapCA.prefab.Length - 1);
```

Explanation:
- Game Day 1 (day=1) uses prefab array index 0
- Game Day 2 (day=2) uses prefab array index 1
- If game day exceeds array length, uses the last element

Examples:
- `Panel1|Panel2|Panel3`: First 3 days show different panels, Day 4+ shows Panel3
- `StorePanel|StorePanel`: Always shows StorePanel

### Step 5: Test Your New Scene

1. Save map.csv file
2. Run the game in Unity
3. Access the new scene through the map selection system
4. Or load it programmatically:
   ```csharp
   SceneLoadManager slm = CBus.Instance.GetManager(ManagerName.SceneLoadManager) as SceneLoadManager;
   slm.Load(1800010); // Use your map ID
   ```

## Scene Loading Mechanism

### Three SceneLoadManager Loading Methods

1. **Load by Scene Name**:
   ```csharp
   slm.Load("Forest"); // Loads scene only, no prefab
   ```

2. **Load by MapCA Object**:
   ```csharp
   MapCA mapData = ...; // Get map configuration object
   slm.Load(mapData); // Loads scene and prefab
   ```

3. **Load by Map ID** (Recommended):
   ```csharp
   slm.Load(1800010); // Automatically fetches config from MapFactory and loads
   ```

### Loading Process

1. Unload current scene and prefab objects
2. Load new scene using Additive mode
3. Select corresponding prefab based on game day
4. Open prefab panel through UIManager

## Example: Complete Process to Add a New Scene

Let's add a "Castle" scene:

### 1. Unity Scene
- Create scene: `Assets/Scenes/Map/Castle.unity`
- Add scene elements, lighting, cameras, etc.
- Add to Build Settings

### 2. Create Prefab
- Create `Assets/Resources/CastlePanel.prefab`
- Design UI interface

### 3. Add CSV Configuration
```csv
1800011,Magic Castle,3,8|9|10|11|12|13|14|15|16|17|18|19|20,Castle,CastlePanel|CastlePanel,UI/Map_Castle,Base/Camera_Castle,EntrancePoint,Talking_01,1
```

### 4. Test
Run the game and verify:
- Castle unlocks on Day 3
- Accessible from 8:00-20:00 daily
- UI displays correctly
- Character behavior is normal

## Common Issues

### Q: Scene doesn't appear in map list?
A: Check these points:
- Is map.csv format correct?
- Is ID unique?
- Does scene name match Unity scene file name?
- Has unlock day been reached?

### Q: Black screen after loading scene?
A: Possible causes:
- No active camera in scene
- Incorrect campath
- Scene not added to Build Settings

### Q: Prefab not loading?
A: Check:
- Is prefab path correct (relative to Resources folder)?
- Is prefab placed in Resources folder or subfolder?
- Is prefab field format correct (using | separator)?

### Q: How to make a scene open only at specific times?
A: Use the opentime field with only the hours you want. Examples:
- Night only: `18|19|20|21|22|23|24|1|2|3|4|5|6`
- Day only: `7|8|9|10|11|12|13|14|15|16|17|18`

### Q: How to create scenes that change with game progress?
A: Use prefab arrays with different prefabs for different game days. Example:
```csv
prefab field: DayPanel1|DayPanel2|DayPanel3|DayPanel4|DayPanel5
```
This shows different UI panels for the first 5 days.

## Advanced Tips

### 1. Dynamic Scene Content
Create multiple prefab variants to show different content on different game days:
- Shop inventory changes over time
- Scene decorations change with seasons
- Character dialogue changes with story progression

### 2. Conditional Unlocking
While the unlockday field sets basic unlock date, you can add additional unlock conditions in code.

### 3. Time Period Control
The opentime field controls map availability, allowing you to create:
- Outdoor scenes open only during daytime
- Bars open only at night
- Shops open during specific hours

## Related Code Files

- **Scene Loading**: `Assets/Scripts/Manager/SceneLoadManager.cs`
- **Map Data Structure**: `Assets/Scripts/Game/Tile/MapCA.cs`
- **Map Factory**: `Assets/Scripts/Game/Tile/MapFactory.cs`
- **Configuration File**: `Assets/StreamingAssets/Data/map.csv`

## Reference Existing Maps

You can study existing map configurations:
- **Your Farm** (ID: 1800001) - Basic map
- **Beach** (ID: 1800009) - Time-restricted map
- **Chicken Farm** (ID: 1800002) - Standard shop map

Happy scene creation! 🎮
