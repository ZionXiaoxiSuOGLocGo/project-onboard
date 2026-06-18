# General Project Analysis Rules (Fallback)

Used when no specific project type is detected.

## Scan Steps

### 1. Map Top-Level Structure
```bash
glob * (or read the root directory)
```
Identify what each top-level file/directory suggests about the project.

### 2. Find Build/Language Clues
Look for these files in order:
| File | Implies |
|---|---|
| `package.json` | Node.js/JavaScript |
| `tsconfig.json` | TypeScript |
| `requirements.txt` or `pyproject.toml` | Python |
| `Cargo.toml` | Rust |
| `go.mod` | Go |
| `pom.xml` or `build.gradle` | Java |
| `CMakeLists.txt` | C/C++ |
| `Makefile` | C or general build |
| `Dockerfile` | Containerized |
| `.sql` files or `migrations/` | Database-focused |
| `.glsl` or `.hlsl` files | Shader/graphics |
| `.csproj` or `.sln` | .NET/C# (non-Unity) |
| `index.html` | Static web |

### 3. Find Entry Points
```bash
grep "main" in top-level files (package.json, Makefile, etc.)
```
```bash
glob **/main.* or glob **/index.* or glob **/app.* or glob **/server.*
```

### 4. Read Key Files
Read these in priority order (30-80 lines each, stop after 5 files):
1. README or README.md
2. The main entry file found in step 3
3. Package/dependency manifest
4. Top-level config file (.env, config.js, settings.py)
5. Directory with most files (likely the main source)

### 5. Categorize
Based on what was found, classify the project type:
- Web app (frontend + backend)
- CLI tool
- Library/SDK
- Database/SQL project
- Graphics/Shader project
- Game (unknown engine)
- Other (describe briefly)

## AGENTS.md Output
Follow the standard template. In "Notes", flag anything unusual and suggest the user create a type-specific rule pack if they work with this project type frequently.
