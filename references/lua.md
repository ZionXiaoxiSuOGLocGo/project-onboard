# Lua Project Analysis Rules

## Signature Detection
- `*.rockspec` files exist
- `lua_modules/` directory
- `.luacheckrc` or `.busted` files
- `lua/` directory with predominantly `.lua` files in top-level
- `init.lua` as main module entry pattern

## Scan Steps

### 1. Read Project Manifest
Check in order of preference:
1. `*.rockspec` → LuaRocks package (name, version, dependencies)
2. `Makefile` → Common in C+Lua hybrid projects
3. `CMakeLists.txt` → Embedded Lua in C/C++ projects
4. `lua_modules/` → Vendored LuaRocks dependencies

Extract key frameworks from rockspec dependencies:
- `lua >= 5.1` / `lua >= 5.4` → Lua version target
- `luajit` → LuaJIT target
- `busted` → BDD test framework
- `luacheck` → Static analysis / linting
- `luaunit` → Unit testing
- `luasocket` → Networking
- `lpeg` → Parsing expression grammar
- `middleclass` / `classic` / `30log` → OOP class system
- `lua-cjson` / `dkjson` → JSON handling
- `penlight` → Utility library
- `moonscript` → Moonscript (compiles to Lua)

### 2. Detect Project Type
| Clues | Type |
|---|---|
| `main.lua` + `conf.lua` | LÖVE2D game |
| `init.lua` in scripts dir | Neovim plugin |
| `.toc` file | World of Warcraft Addon |
| `nginx.conf` + Lua files | OpenResty / Nginx module |
| `kong/` directory | Kong API Gateway plugin |
| `lapis` in deps | Lapis web framework |
| `lua-resty-*` deps | OpenResty library |
| `hammerspoon/` or `.hammerspoon/` | Hammerspoon config |
| `awesome/` directory | Awesome WM config |
| C/C++ files + Lua files | Embedded Lua host (game modding, Redis module, etc.) |
| `tarantool` | Tarantool app |
| `moonscript` | Moonscript project |

### 3. Map Directory Structure
| Directory | Common Role |
|---|---|
| `src/` or `lib/` | Library source code |
| `lua/` | Lua source (often in hybrid projects) |
| `spec/` or `test/` | Test suite (busted, luaunit) |
| `examples/` | Usage examples |
| `bin/` | Executable scripts |
| `rocks/` | LuaRocks tree |
| `lua_modules/` | Vendored dependencies |
| `doc/` or `docs/` | Documentation (often LDoc) |
| `assets/` | Game assets (LÖVE2D) |
| `conf/` | Configuration modules |
| `vendor/` | Third-party dependencies |

### 4. Find Entry Points
```bash
grep "^require\|^module\|dofile" *.lua
```
Project-type specific:
- LÖVE2D: `main.lua` → entry point, `conf.lua` → config
- Neovim plugin: `after/plugin/<name>.lua` or `plugin/<name>.lua`
- WoW Addon: `.toc` file lists loaded Lua files in order
- OpenResty: `init.lua` or `app.lua` required from `nginx.conf`
- Hammerspoon: `~/.hammerspoon/init.lua`
- General Lua module: `M = {}; return M` or `package.loaded['...']`

### 5. Check Module System
Lua has no built-in module system. Check which pattern is used:
- `require "mod"` → Built-in require
- `luarocks.loader` → LuaRocks loader
- `package.path` / `package.cpath` overrides
- Custom `require` wrappers (common in game engines)
- `import` → Moonscript

### 6. Check for Config Files
- `.luacheckrc` → Linting rules and globals configuration
- `.busted` → Test framework config
- `.luacov` → Coverage config
- `rockspec` / `rockspecs/` → LuaRocks packaging
- `Makefile` → Build/test commands
- `.travis.yml` / `.github/workflows/` → CI/CD
- `ldoc.lua` or `config.ld` → LDoc documentation config
