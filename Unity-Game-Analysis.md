# Unity Game Analysis Report

## Game Overview

This is a **Farm Simulation Game** developed with Unity Engine, combining time management, resource management, character interaction, and story progression gameplay elements.

## Game Type

**Main Type:** Farm Simulation Game  
**Sub-types:** Time Management + Resource Management + Character Interaction + Story Progression

## Core Game Mechanics

### 1. Time System
- **Day-Night Cycle**: The game uses a dual time system with days and hours
- Each day has 24 hours, starting from hour 18 (6 PM)
- Player actions consume time (e.g., talking to NPCs costs 1 hour)
- When time falls below 3 hours, night dialogue triggers and advances to the next day
- Each day ends with specific daily events and dialogues

### 2. Farm Management System
The core gameplay revolves around farm land management:

#### Land States
- **Uncultivated**: Initial state
- **Empty**: Plowed but not watered
- **Wet**: Watered and ready for planting

#### Farm Operations
- **Hoeing**: Use hoe to cultivate uncultivated land
- **Watering**: Water empty land to make it wet
- **Planting**: Plant seeds (e.g., grass seeds) on wet or empty land
- **Growth Cycle**: Crops have a growth process counter that decreases each day
- **Harvesting**: Automatic harvest when process reaches zero

Players start with 9 plots of land to manage.

### 3. Card System
The game features a unique card-based interaction system:

#### Card Types
- **Asset Cards**: Represent items, tools, currency
- **Character Cards**: Represent NPC characters
- **Furniture Cards**: Represent decorative or functional furniture

#### Card Functions
- Drag & drop placement to specific slots (FillItem)
- Left-click to show information
- Right-click to trigger events
- Ownership concept (player-owned vs NPC-owned)

### 4. Resource Management System

#### Available Resources:
- **Copper Coins** (ID: 1100001) - Game currency, value 10
- **Grass Seeds** (ID: 1100002) - Can be planted to grow grass
- **Grass** (ID: 1100003) - Used to feed horses
- **Hoe** (ID: 1100004) - Farming tool with durability (20 uses)

The resource system supports:
- Item quantity tracking
- Adding/removing items
- Special items (ID 2000-3000) use model instantiation

### 5. Map System
The game includes multiple visitable map locations:

- **Your Farm** (ID: 1800001)
- **Chicken Farm** (ID: 1800002)
- **General Store** (ID: 1800003)
- **Ranch** (ID: 1800004)
- **Hospital** (ID: 1800005)
- **Library** (ID: 1800006)
- **Mine** (ID: 1800007)
- **Church** (ID: 1800008)
- **Beach** (ID: 1800009, opens at specific hours: 8-20)

Each map has unlock conditions, opening hours, and specific scenes.

### 6. Character Interaction System

#### NPC Characters
- **Girl** (ID: 1200001)
- **Mayor Kane** (ID: 1200002)
- **Ranch Owner Li Beizhen** (ID: 1200003)

#### Dialogue System
- Uses XNode graph-based node system for dialogue trees
- Dialogues dynamically generated based on location, character, and game day
- Talking to characters costs time (1 hour)
- Dialogue content is saved; repeated conversations show previous content
- Supports branching choices and event triggers

### 7. Work System
The game includes a job mechanism:
- Players can accept work tasks
- Jobs have start times and end times
- Completing work rewards items
- Work periods feature specific alerts and work dialogues

### 8. Level/Experience System
- Player has Level and Experience (Exp) system
- Experience growth triggers level advancement
- UI displays experience bar showing current progress

### 9. Story System
- Day-by-day story progression (each day has specific dialogues)
- Opening dialogue at game start
- Morning wake-up dialogue each day ("Day N Wake Up")
- Night rest dialogue ("NightDialog")

## Technical Features

### Technologies and Plugins Used
1. **DOTween/DOTweenPro** - Animation and tweening effects
2. **XNode** - Visual node editor (for dialogue system)
3. **Universal Render Pipeline (URP)** - Unity's render pipeline
4. **NavMesh Components** - AI pathfinding system
5. **SpringBone** - Spring bone animation system
6. **Custom Framework** - RG.Zeluda framework using manager pattern

### Code Architecture
- **Manager Pattern**: Various managers (GameManager, DialogManager, UIManager, etc.)
- **Factory Pattern**: For creating game objects (CardFactory, AssetFactory, etc.)
- **CA (Content Asset) System**: Data configuration classes
- **CSV Data-Driven**: Game content configured through CSV files
- **Event System**: Event-based game logic communication

## Game Flow

### Game Start
1. Show opening dialogue ("Game Start")
2. Initialize main panel (MainPanel)
3. Create 9 farm plots
4. Set initial time to Day 1, Hour 18

### Daily Loop
1. **Morning**: Play "new day begins" sound, show wake-up dialogue
2. **Daytime**: Players can freely:
   - Explore different maps
   - Talk to NPCs (costs time)
   - Perform farm operations (plowing, watering, planting)
   - Execute work tasks
3. **Night**: When time drops below 3 hours, triggers night dialogue and advances to next day

### Farm Operation Flow
1. Use hoe to cultivate land
2. Water the land
3. Plant seeds (e.g., grass seeds)
4. Wait for crops to mature (3 days)
5. Automatic harvest yields crops

## Game Design Features

1. **Card-Based Interaction**: Items and characters presented as draggable cards
2. **Time Management**: Players must plan daily time allocation wisely
3. **Progressive Unlocking**: New maps and content unlock as game progresses
4. **Social Interaction**: Build relationships with multiple NPCs, advance story
5. **Resource Loop**: Plant → Harvest → Feed → Produce, forming complete resource cycle
6. **Visual Clock**: UI features a 24-hour circular clock showing time passage

## Game Objectives

Based on code analysis, the game appears to be a life simulation centered on farm management where players need to:
- Manage their own farm
- Build relationships with town residents
- Complete various jobs to earn resources
- Explore different map locations
- Progress through the main storyline

## Current Development Status

From the code, the game is in active development with:
- Core system framework completed
- Some functions still empty or commented out
- Complete data configuration system
- UI system largely finished
- Some features still to be implemented (e.g., video panel)

## Summary

This is a **Farm Simulation Game** where players take the role of a farm owner, managing their farm in a small town, interacting with NPCs, and progressing through time and resource management. The game uses card-based interaction design, combining traditional farm management gameplay with social simulation elements, offering strong playability and development potential.

The core fun factors include:
- **Management Achievement**: Watching your farm grow from wasteland to prosperity
- **Time Planning**: Strategically scheduling daily activities
- **Character Interaction**: Building relationships with NPCs, unlocking storylines
- **Exploration**: Discovering different maps and locations
- **Resource Management**: Balancing acquisition and use of various resources

This is a game suitable for players who enjoy casual simulation, farm management, and character development genres.
