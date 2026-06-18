# C/C++ Project Analysis Rules

## Signature Detection
- `CMakeLists.txt` or `Makefile` exists at root
- `.cpp` / `.c` / `.h` / `.hpp` files present

## Scan Steps

### 1. Read Build System

**CMake (CMakeLists.txt)** — preferred, most common:
- `cmake_minimum_required(VERSION ...)` → CMake version
- `project(<name> VERSION ... LANGUAGES CXX C)` → Project identity
- `add_executable(...)` → Binary targets
- `add_library(...)` → Library targets
- `find_package(...)` → External dependencies. Key packages:
  - `Qt5` / `Qt6` → Qt GUI framework
  - `OpenGL`, `Vulkan`, `SDL2`, `GLFW` → Graphics/game
  - `Boost`, `OpenCV`, `Eigen` → Scientific/compute
  - `protobuf`, `gRPC` → API/RPC
  - `CURL`, `OpenSSL` → Networking
  - `Doxygen` → Documentation generator
- `target_link_libraries(...)` → Which libs link to what

**Makefile** — simpler, less structured:
- Look for `CC`, `CXX`, `CFLAGS`, `LDFLAGS`
- `all:` target → what gets built by default

### 2. Determine Project Type
| Clues | Type |
|---|---|
| `QApplication` or `QWidget` in source | Qt application |
| `glfw`, `glad`, `glew`, `sdl` dependency | Graphics/game |
| `main()` with `while(running)` loop | Game engine or real-time app |
| `WinMain` for Windows | Windows native app |
| `src/` + `include/` layout | Library |
| CLI-only with no GUI deps | Command-line tool |
| Embedded build tool (PlatformIO, Arduino) | Embedded firmware |
| `vulkan` or `opengl` dependency | Graphics engine |
| CUDA in CMake (`find_package(CUDA)`) | GPU compute |

### 3. Map Directory Structure
Common C++ project layouts:

**Classic (many projects):**
```
src/          → Source files (.cpp, .c)
include/      → Public headers (.h, .hpp)
lib/          → External libraries
test/         → Test code
build/        → Build output (ignore for analysis)
third_party/  → Vendored dependencies
docs/         → Documentation
```

**Modern CMake (header-only or module):**
```
libs/<name>/
  include/<name>/  → Public headers
  src/              → Implementation
  CMakeLists.txt    → Library build
apps/<name>/
  src/              → Application
  CMakeLists.txt    → App build
```

**Game/Graphics engine:**
```
Source/        → Engine core
Editor/        → Editor code (separate target)
Shaders/       → GLSL/HLSL files
Assets/        → Game assets
Config/        → Engine config
```

### 4. Find Entry Points
```bash
grep "int main(" in src/ or *.cpp *.c
```
```bash
grep "WinMain" in src/ or *.cpp  (Windows)
```
For Qt: look for `QApplication` + `exec()` call.

### 5. Identify Architecture
- **Object-oriented**: Class hierarchies, virtual functions, inheritance
- **Data-oriented (ECS)**: Entity/Component/System pattern, small POD structs
- **Procedural**: Flat functions, structs without methods (common in C)
- **RAII pattern**: Smart pointers (`unique_ptr`, `shared_ptr`), constructors/destructors
- **Singleton**: `static` local or Meyer's singleton pattern
- **Observer pattern**: Callback lists, signals/slots (especially Qt)

### 6. Check Config Files
- `.clang-format` → Code formatting rules
- `.clang-tidy` → Static analysis rules
- `Doxyfile` → Documentation config
- `conanfile.txt` or `vcpkg.json` → Package manager
- `.github/workflows/` → CI configuration
- `.editorconfig` → Editor settings

### 7. Build & Test Commands
**CMake:**
```bash
mkdir build && cd build
cmake ..
cmake --build .           # or make (on Linux/Mac)
ctest                     # run tests
```

**Makefile:**
```bash
make          # build
make test     # test (if target exists)
make clean    # clean
```

**Cross-platform note**: Build commands differ on Windows (Visual Studio/MSBuild) vs Linux/Mac (make/ninja). Ask the user for their build setup if CMake isn't clear.

### 8. Identify Headers vs Sources
C/C++ typically separates declaration (`.h`/`.hpp`) from implementation (`.c`/`.cpp`). This is important for the AI agent to know when analyzing the codebase:
- Read headers to understand the API contract
- Read sources to understand implementation details
- `.cppm` files → C++20 modules (new, less common)
