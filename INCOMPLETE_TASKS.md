# Incomplete Tasks List

## Project Overview
This document lists all the parts in the GameDevSolitaireAlfafarDev project that need to be implemented or completed.
This project is for the 0th Indie Pointer Game Development Solitaire Jam, a Unity-based text adventure game.

---

## I. Core System Classes - Empty or Minimal Implementation

### 1. GameCenter (Logic/GameCenter.cs)
**Status**: Only has an empty Save method
**Needs Implementation**:
- Actual save game logic in Save() method
- May need Load() method
- Game state management

### 2. ActionManager (Logic/Manager/ActionManager.cs)
**Status**: Completely empty Manager class
**Needs Implementation**:
- Action system management logic
- All action-related functionality

---

## II. UI Panel Classes - Empty Implementation

### 3. VideoPanel (Scripts/Panel/Video/VideoPanel.cs)
**Status**: Only defines VideoPlayer field, no logic
**Needs Implementation**:
- Video playback control logic
- Panel show/hide logic

### 4. BurnPanel (Scripts/Panel/Burn/BurnPanel.cs)
**Status**: Completely empty Panel class
**Needs Implementation**:
- Burning/smelting related UI logic
- Gameplay interaction

---

## III. Game Logic Classes - Empty Implementation

### 5. CardManager (Scripts/Game/Control/CardManager.cs)
**Status**: Only has CardPanel field, no management logic
**Needs Implementation**:
- Card management system
- All card operation functionality

### 6. TradeTable (Scripts/Game/Furniture/TradeTable.cs)
**Status**: OpenEventPanel method is empty
**Needs Implementation**:
- Trade table event panel opening logic
- Trading functionality

---

## IV. Data Classes - Empty or Minimal Implementation

### 7. ProductData (Logic/Data/Farm/ProductData.cs)
**Status**: Completely empty data class
**Needs Implementation**:
- All product data fields and properties
- Data structure definition

### 8. CardCA (Scripts/Game/Card/CardCA.cs)
**Status**: Only has resPath field
**Needs Implementation**:
- Complete card configuration data
- Other necessary properties

### 9. ParameterCA (Scripts/Game/Parameter/ParameterCA.cs)
**Status**: Only has value field
**Needs Implementation**:
- Complete parameter configuration data
- May need name, description, and other fields

### 10. Parameter (Scripts/Game/Parameter/Parameter.cs)
**Status**: Only has constructor, no actual implementation
**Needs Implementation**:
- Parameter business logic
- Parameter usage and operation methods

### 11. EventsCA (Scripts/Game/Events/EventsCA.cs)
**Status**: Empty configuration class
**Needs Implementation**:
- Event configuration data structure

### 12. CharacterCA (Scripts/Game/Character/CharacterCA.cs)
**Status**: Only has path field
**Needs Implementation**:
- Complete character configuration data
- Character attributes, states, etc.

### 13. FurnitureCA (Scripts/Game/Furniture/FurnitureCA.cs)
**Status**: Empty configuration class
**Needs Implementation**:
- Furniture configuration data structure

### 14. MapCA (Scripts/Game/Tile/MapCA.cs)
**Status**: Empty configuration class
**Needs Implementation**:
- Map configuration data structure

### 15. WorkCA (Scripts/Game/Work/WorkCA.cs)
**Status**: Empty configuration class
**Needs Implementation**:
- Work configuration data structure

### 16. DailyCA (Scripts/Daily/DailyCA.cs)
**Status**: Empty configuration class
**Needs Implementation**:
- Daily system configuration data

---

## V. Skill System - Empty Implementation

### 17. SkillBase (Logic/Skill/SkillBase.cs)
**Status**: Cast method is empty
**Needs Implementation**:
- Actual skill casting logic
- Skill effect system

### 18. HoeSkill (Logic/Skill/HoeSkill.cs)
**Status**: Needs verification if implemented
**Needs Implementation**:
- Specific hoe skill logic

---

## VI. UI Item Classes - Minimal Implementation

### 19. EventItem (Scripts/Panel/Event/EventItem.cs)
**Status**: 11-line minimal implementation
**Needs Implementation**:
- Complete event item UI logic

### 20. BegItem (Scripts/Panel/Main/BegItem.cs)
**Status**: 12-line minimal implementation
**Needs Implementation**:
- Begging item UI logic

### 21. LineItem (Logic/Panel/LineItem.cs)
**Status**: 15-line minimal implementation
**Needs Implementation**:
- Line item UI logic

---

## VII. Manager Classes - Minimal Implementation

### 22. MapManager (Scripts/Game/Tile/MapManager.cs)
**Status**: 11-line minimal implementation
**Needs Implementation**:
- Complete map management logic

### 23. GameEntrance (Scripts/Manager/GameEntrance.cs)
**Status**: 18-line minimal implementation
**Needs Implementation**:
- Complete game entrance initialization logic

---

## VIII. Dialogue System

### 24. DialogGraph (Scripts/Dialog/DialogGraph.cs)
**Status**: 8-line minimal implementation
**Needs Implementation**:
- Complete dialogue graph logic
- Dialogue flow control

---

## IX. Factory Classes - Minimal Implementation

### 25. DailyFactory (Scripts/Daily/DailyFactory.cs)
**Status**: 19-line minimal implementation
**Needs Implementation**:
- Daily system factory creation logic

### 26. ParameterFactory (Scripts/Game/Parameter/ParameterFactory.cs)
**Status**: 18-line minimal implementation
**Needs Implementation**:
- Parameter system factory creation logic

---

## X. Other Features to be Completed

### 27. Scene Numbering System
**Status**: Mentioned in activity requirements
**Needs Implementation**:
- Add scene numbers to each scene
- Mark developer information in the "Scene ID Table"
- For subsequent scoring statistics

### 28. Technical Documentation
**Status**: Optional as mentioned in activity requirements
**Recommended**:
- Add technical documentation for complex features
- Help subsequent developers understand implementation logic

---

## XI. Discovered TODO Comments

### 29. XNode Inheritance Issue
**Location**: `ProjectUnity/Client/Assets/XNode/Scripts/Node.cs:343`
**Content**: 
```csharp
// TODO: Make inheritance work in such a way that applying [DisallowMultipleNodes(1)] to type NodeBar : Node
```
**Description**: Unresolved issue about node inheritance in XNode plugin

### 30. EditorGUIEx Rewrite
**Location**: `ProjectUnity/Client/Assets/Lib/EditorExtend/Editor/EditorGUIEx.cs:38`
**Content**: 
```csharp
* \todo Ugly, rewrite this class at some point...
```
**Description**: Editor extension class needs refactoring

---

## XII. NotImplementedException

The following classes have NotImplementedException, indicating unimplemented functionality:

### 31. PartStream (Assets/Lib/RGBase/PartStream.cs)
- `CanSeek` property
- `CanWrite` property
- Several other stream operation methods

### 32. ResLoaderEditor (Assets/Lib/Framework/ResLoader/ResLoaderEditor.cs)
- Editor resource loading related functionality

---

## Summary

### Categorized by Priority:

#### High Priority (Affects core gameplay):
1. GameCenter - Save system
2. ActionManager - Action management
3. CardManager - Card management
4. SkillBase - Skill system
5. DialogGraph - Dialogue system

#### Medium Priority (Affects game experience):
1. Various Panel classes (VideoPanel, BurnPanel, etc.)
2. TradeTable - Trading functionality
3. MapManager - Map management
4. Various CA configuration classes

#### Low Priority (Data and UI details):
1. Various Item classes
2. Factory class supplementation
3. Data class improvements

#### Optional:
1. Technical documentation
2. Scene numbering annotation
3. EditorGUIEx refactoring

---

## Development Recommendations

1. **Prioritize core systems**: Start with core classes like GameCenter, ActionManager
2. **Configuration first**: Complete various CA configuration classes, define data structures
3. **Incremental implementation**: Implement each Panel and Manager according to game flow
4. **Test and verify**: Test promptly after completing each module
5. **Sync documentation**: Add comments and documentation for complex logic

---

**Generated**: 2026-02-16
**Project**: GameDevSolitaireAlfafarDev
**Event**: 0th Indie Pointer Game Development Solitaire Jam
