<!--
SYNC IMPACT REPORT
==================
Version: 0.0.0 → 1.0.0
Change Type: MAJOR (Initial constitution establishment)
Rationale: First formal constitution for CLI application project
Date: 2026-01-30

Principles Defined:
  - I. Robust Command-Line Parsing (NEW)
  - II. Zero-Dependency Distribution (NEW)
  - III. Embedded Documentation (NEW)
  - IV. User Experience Excellence (NEW)
  - V. Structured I/O & Composability (NEW)
  - VI. Intuitive Grammar (NEW)
  - VII. Configuration & Extensibility (NEW)

Templates Requiring Updates:
  ✅ plan-template.md - Validated (CLI-specific constraints align)
  ✅ spec-template.md - Validated (User story format compatible)
  ✅ tasks-template.md - Validated (Task organization supports CLI principles)

Follow-up TODOs: None
-->

# CLI Application Constitution

## Core Principles

### I. Robust Command-Line Parsing

MUST use established, full-featured command-line parsing frameworks such as cobra (Go), clap (Rust), click (Python), or equivalent libraries in other languages. Hand-rolled argument parsing is PROHIBITED for production features.

**Rationale**: Modern CLI frameworks provide battle-tested implementations of subcommands, flag handling, autocompletion generation, and help text formatting. Reinventing this infrastructure introduces bugs, maintenance burden, and inconsistent user experience. Framework selection ensures predictable behavior and reduces development time.

**Requirements**:
- Framework must support subcommands and nested command hierarchies
- Framework must enable autocompletion script generation for multiple shells
- Framework must provide built-in help text and usage formatting
- All command definitions must be declarative and self-documenting

### II. Zero-Dependency Distribution

Applications MUST be distributable as single files with no external runtime dependencies. Users should be able to download and execute the tool immediately without installing language runtimes, package managers, or dependency trees.

**Rationale**: Adoption barriers kill useful tools. Every dependency installation step loses 50%+ of potential users. Go and Rust excel at static binaries; Python/Node.js tools must provide executable packages (PyInstaller, pkg) or containerized alternatives with clear run instructions.

**Requirements**:
- Primary distribution method: single binary or executable
- Secondary distribution: Docker image with documented one-liner execution
- Tertiary distribution: language package manager (npm, pip) for developer users
- All external dependencies must be bundled or statically linked
- Installation must not require system-level package manager access

### III. Embedded Documentation

Documentation MUST be embedded within the tool itself, accessible without internet connection or external files. The tool must be self-explanatory through built-in help commands, examples, and usage guidance.

**Rationale**: Developers use CLI tools in diverse environments: air-gapped systems, unreliable networks, containers without volume mounts. External wikis become outdated and inaccessible. Self-contained documentation ensures consistent, version-matched guidance at the point of use.

**Requirements**:
- Every command must provide `--help` flag with detailed usage information
- Help text must include practical examples for common use cases
- Tool must provide command to display principles of operation and architecture
- Tool must support discovery of available functionality (verb listing, introspection)
- Consider markdown rendering for rich documentation display (glamour, rich)
- Examples must cover basic, intermediate, and advanced usage patterns

### IV. User Experience Excellence

User interface must be polished, intuitive, and visually clear. Terminal output should leverage color, formatting, and interactive elements where appropriate, while remaining functional in constrained environments.

**Rationale**: Professional polish signals quality and attention to detail. Color-coded output reduces cognitive load; interactive modes improve complex workflows. However, compatibility with pipes, CI/CD systems, and accessibility tools is non-negotiable.

**Requirements**:
- Output must detect TTY and disable colors/formatting when piping
- Color schemes must be accessible (colorblind-safe, high contrast)
- Progress indicators for long-running operations (spinners, progress bars)
- Interactive modes (TUI) must be opt-in and provide equivalent non-interactive paths
- Error messages must be actionable with clear resolution steps
- Consider tools: bubbletea (Go), rich/textual (Python), lipgloss (Go styling)

### V. Structured I/O & Composability

Tools MUST support structured input/output formats alongside human-readable formats. All commands should be composable with Unix pipes and scriptable in automation contexts.

**Rationale**: CLI tools serve both humans and machines. Structured output enables data pipelines, monitoring integration, and cross-tool orchestration. stdin/stdout protocol ensures composability with existing Unix toolchain and CI/CD systems.

**Requirements**:
- Default output: human-readable, formatted for terminal display
- JSON output: `--output json` or `-o json` flag for all data commands
- CSV/TSV output: `--output csv` for tabular data
- All output formats must be schema-stable (no breaking changes without version bump)
- Input must support structured formats: JSON files, stdin piping
- Errors and diagnostics must go to stderr (never stdout)
- Exit codes must follow conventions: 0=success, 1=error, 2=usage error
- Consider: SQLite export for large datasets, YAML for configuration

### VI. Intuitive Grammar

Command structure must follow consistent, predictable patterns. Verb/noun organization should make command discovery intuitive and reduce the need for documentation lookups.

**Rationale**: Users build mental models of CLI tools based on their grammar. Inconsistent patterns (e.g., `get --update` to set values, or mixing `ls-files` and `hosts list`) force memorization and create friction. Natural language patterns reduce cognitive load.

**Requirements**:
- Use consistent verb patterns: `get`/`set`, `list`/`create`/`delete`, `start`/`stop`
- Resource nouns should be consistent across commands (e.g., `files`, `hosts`, `config`)
- Avoid mixing styles: choose either `verb-noun` or `noun verb` and apply uniformly
- Flags should have consistent meanings across commands (`--force`, `--dry-run`, `--verbose`)
- Plan for iteration: grammar will evolve through usage feedback
- Document grammar patterns in help text and architecture documentation

### VII. Configuration & Extensibility

Tools MUST provide flexible configuration mechanisms and plugin extensibility. Users should be able to customize behavior through multiple layers of settings and extend functionality without modifying source code.

**Rationale**: Different environments require different defaults. Local development, CI/CD, and production contexts need distinct configurations. Plugin architecture enables community contributions and organization-specific customizations without forking.

**Requirements**:
- Environment variables for all configurable settings (CI/CD friendly)
- Configuration file support (JSON, YAML, or TOML) with layered precedence:
  1. Command-line flags (highest priority)
  2. Environment variables
  3. Local config file (project-specific, not in VCS)
  4. Global config file (user home directory)
  5. Built-in defaults (lowest priority)
- Provide `config` subcommand to query, set, and validate configuration
- Plugin system: lookup `toolname-XXX` binary for unknown subcommand `XXX`
- Shell autocompletion for commands, flags, and dynamic values when feasible
- Configuration schema must be documented and version-stable

## Development Standards

All CLI applications must adhere to the following technical and process standards:

**Code Quality**:
- Test coverage MUST exceed 80% for core command logic
- Integration tests MUST cover all primary user workflows
- Error handling must be comprehensive with user-friendly messages
- Logging must support structured formats (JSON) for operational visibility

**Documentation**:
- README must include installation instructions for all supported platforms
- CHANGELOG must follow semantic versioning and document all user-facing changes
- Architecture documentation must explain command organization and extension points
- Examples must be tested as part of CI/CD pipeline

**Distribution**:
- GitHub Releases with binaries for: Linux (amd64, arm64), macOS (Intel, Apple Silicon), Windows (amd64)
- Checksums (SHA256) for all released binaries
- Installation script for Unix systems: `curl -sSL https://... | sh`
- Optional: Package manager integration (brew, apt, snap, chocolatey)

## Governance

This constitution is the authoritative source of truth for CLI application development practices. All code reviews, design decisions, and feature implementations must demonstrate compliance with these principles.

**Amendment Process**:
- Proposals must document rationale and affected systems
- MINOR version bump: New principle added or clarification that doesn't invalidate existing code
- MAJOR version bump: Principle change that requires code refactoring or removes requirements
- PATCH version bump: Typo fixes, wording improvements, formatting

**Compliance**:
- Pull requests must reference constitution principles in design rationale
- Code reviews must verify principle adherence before approval
- Technical debt that violates principles must be tracked with remediation timeline
- For AI agents and developers: consult this constitution before making architectural decisions

**Version**: 1.0.0 | **Ratified**: 2026-01-30 | **Last Amended**: 2026-01-30
