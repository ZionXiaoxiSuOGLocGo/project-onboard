# project-onboard 更新日志

## 2026-06-21 — v1.0.1 紧急修复
**来源**：ThirdPersonTest 项目实际使用反馈

### 变更内容摘要
| 所在章节 | 新增/修改内容 |
|---|---|
| YAML Frontmatter | 修复 Copyright 声明导致的格式解析失败（移至 `---` 闭合标签之后） |
| 部署方式 | 修复 `.git/` 目录导致 opencode 跳过 skill 的问题。建立双目录工作流：`~/OpenCode_skills/`（开发+Git 管理）↔ `~/.config/opencode/skills/`（安装，无 .git） |

### 更新原则
- 所有 text 文件头部不能有非 YAML 内容出现在 `---` frontmatter 之前
- skill 安装目录不可包含 `.git/` 文件夹

---

## 2026-06-19 — v1.0.0 初始发布
**来源**：openSkills 项目启动

### 变更内容摘要
| 所在章节 | 新增/修改内容 |
|---|---|
| 核心逻辑 | 5 步执行流：确认目标 → 检测项目类型（12 种签名匹配） → 加载规则包 → 深度扫描 → 生成 AGENTS.md |
| 规则包 | 9 个完整规则文件：Unity (包含 .meta/Prefab 分析)、Unreal (模块依赖+框架类)、Node.js (前端框架识别)、Python (Web/ML/CLI 分类)、Rust (crate 类型检测)、Go (标准布局+架构模式)、Java (Maven/Gradle+Spring Boot)、C/C++ (CMake+Qt)、General (通用回退) |
| README | 竞争对比表（vs Understand-Anything：9 项优势对比）、安装说明、9 种支持类型表 |
| 许可证 | GPL-3.0 |
| Copyright | ZionXiaoxiSuOGLocGo |

### 更新原则
- 可插拔规则包——新增类型只需一个 md 文件，不动核心逻辑
- AGENTS.md 面向 AI 代理优化（表格+列表、200-400行），非人类文档
- 零外部依赖，仅用 opencode 内置工具（glob/grep/read）
- 自动检测优先，支持 `--type` 手动覆盖
