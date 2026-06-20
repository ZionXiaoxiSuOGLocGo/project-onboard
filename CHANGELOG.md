# Changelog

## v1.0.1 (2026-06-21)

### Fixed
- **Skill not loading in opencode**: `Copyright` and `SPDX` lines placed above the YAML `---` frontmatter delimiter broke opencode's SKILL.md parser. Moved copyright to after the YAML block.
- **`.git/` directory blocking skill discovery**: Running `git init` inside the skills directory created a `.git/` folder that caused opencode to skip the skill entirely. Removed `.git/` from `~/.config/opencode/skills/project-onboard/` and established a dual-directory workflow (development at `~/OpenCode_skills/project-onboard/`, install target at `~/.config/opencode/skills/project-onboard/`).

---

## v1.0.0 (2026-06-19)

### Added
- Initial release: lightweight, zero-dependency project onboard skill for AI coding agents.
- Auto-detection of 9 project types (Unity, Unreal, Node.js, Python, Rust, Go, Java, C/C++, General).
- 9 complete rule packs with per-type analysis instructions.
- GPL-3.0 license.
- README.md with competitive advantages comparison table vs Understand-Anything.
- Installation instructions and usage examples.
- Copyright notice.
