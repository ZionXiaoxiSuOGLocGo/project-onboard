# Unreal Engine Project Analysis Rules

## Signature Detection
- `Source/` directory exists
- `.uproject` file in root
- `.Build.cs` files in module directories

## Scan Steps

### 1. Read Project Metadata
Read the `.uproject` file (JSON format). Extract:
- `EngineAssociation` — Engine version (e.g. "5.4")
- `Modules[]` — Project modules (names, types: Runtime/Editor)
- `Plugins[]` — Enabled plugins

### 2. Read Module Dependencies
For each module directory under `Source/`, read the `.Build.cs` file:
- `PublicDependencyModuleNames` — Public API dependencies
- `PrivateDependencyModuleNames` — Internal dependencies
Key modules to identify:
- `Core`, `CoreUObject` — Always present
- `Engine`, `UMG` — Gameplay and UI
- `EnhancedInput` — New input system
- `GameplayAbilities` — GAS (Gameplay Ability System)
- `OnlineSubsystem` — Multiplayer
- `Niagara` — VFX
- `Chaos` — Physics
- `Landscape`, `Foliage` — Terrain

### 3. Map Directory Structure
| Directory | Common Role |
|---|---|
| `Source/<ModuleName>/Public/` | Header files (.h) |
| `Source/<ModuleName>/Private/` | Implementation files (.cpp) |
| `Content/` | Assets (Blueprints, Materials, Maps) |
| `Config/` | Engine and game configuration (.ini) |
| `Plugins/` | Additional plugins |
| `Binaries/` | Compiled binaries |
| `Intermediate/` | Build artifacts (ignore) |
| `DerivedDataCache/` | Cached assets (ignore) |

### 4. Find Core Classes
```bash
grep "class.*GameMode\|class.*GameState\|class.*PlayerController\|class.*Pawn\|class.*Character" in Source/
```
Common framework classes:
- `GameMode` — Match rules, spawning, game flow
- `GameState` — Replicated state visible to all players
- `PlayerController` — Player input and camera
- `PlayerState` — Per-player replicated state
- `Pawn` / `Character` — Physical avatar in the world
- `HUD` — On-screen display
- `GameInstance` — Persistent across levels

### 5. Find Entry Points
- Default map: Check `Config/DefaultEngine.ini` for `GameDefaultMap`
- Default GameMode: Check `Config/DefaultEngine.ini` for `GlobalDefaultGameMode`
- Level files: `glob **/*.umap` in `Content/` (binary, can't read directly)

### 6. Check Config Files
- `Config/DefaultEngine.ini` — Engine settings, renderer, input
- `Config/DefaultGame.ini` — Game-specific settings
- `Config/DefaultInput.ini` — Input bindings (if not using EnhancedInput)

### 7. Identify Patterns
- Is GAS (Gameplay Ability System) in use? → `GameplayAbilities` module
- Is Enhanced Input in use? → `EnhancedInput` module + `InputAction` assets
- Is Networking in use? → Check `bReplicates` patterns + OnlineSubsystem
- Blueprint-heavy or C++-heavy? → Compare `Content/` vs `Source/` file counts

## AGENTS.md Additions for Unreal
Include:
- Engine version from `.uproject`
- Modules list with brief purpose
- Key framework classes (GameMode, PlayerController, etc.)
- If UMG is present, note that UI is likely Blueprint-driven
- Note: `.uasset`/`.umap` files are binary — only C++ and config files are readable
