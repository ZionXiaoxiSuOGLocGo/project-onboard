Copyright (C) 2026 ZionXiaoxiSuOGLocGo
SPDX-License-Identifier: GPL-3.0-or-later

# Project Onboard

Auto-detect project type and generate AGENTS.md for AI coding agents. Zero dependencies.

## What It Does

Give your AI agent a project path — it automatically:

1. Scans directory structure to detect the project type
2. Loads the matching rule pack
3. Generates a concise AGENTS.md with architecture, dependencies, entry points, and core modules

Your agent instantly understands the project without you explaining anything.

## Supported Project Types

| Type | Signature |
|---|---|
| Unity | `Assets/` + `ProjectSettings/` |
| Unreal Engine | `Source/` + `.uproject` |
| Node.js / Frontend | `package.json` (no `Assets/`) |
| Python | `pyproject.toml` / `requirements.txt` |
| Rust | `Cargo.toml` |
| Go | `go.mod` |
| Java / Maven / Gradle | `pom.xml` / `build.gradle` |
| C / C++ | `CMakeLists.txt` |
| General | Fallback for anything else |

## Installation

Copy to your skills directory:

```
~/.config/opencode/skills/project-onboard/SKILL.md
~/.config/opencode/skills/project-onboard/references/*.md
```

Also compatible with `.claude/skills/` and `.agents/skills/`.

## Usage

```
# Auto-detect project type
onboard C:\my-unity-project

# Skip detection, force a specific type
onboard C:\my-project --type unity
```

Output: `<project_root>/AGENTS.md`

## Adding New Project Types

1. Add a detection signature in `SKILL.md` Step 2
2. Create `references/<name>.md` following the existing format
3. The rule pack auto-loads on next `onboard` run

## License

GPL-3.0 — see [LICENSE](LICENSE)
