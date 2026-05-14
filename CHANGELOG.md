# Changelog

## [v2.1.138] — 2026-05-09

### Synced to Claude Code v2.1.138

Bumps tutorial coverage from Claude Code v2.1.131 → v2.1.138 (May 9, 2026
release). Anthropic shipped seven patches between v2.1.132 and v2.1.138 since
the last sync.

### Added (English docs)

- `worktree.baseRef` setting (v2.1.133) — controls whether `claude --worktree`
  branches from `origin/<default>` (`"fresh"`, default) or local `HEAD`
  (`"head"`). **Behavior change**: the `"fresh"` default reverts the v2.1.128
  behavior, so users who relied on local-`HEAD` branching after v2.1.128 must
  opt back in. Documented in `09-advanced-features/README.md`.
- `autoMode.hard_deny` admin key (v2.1.136) — array of classifier rules that
  block a class of actions regardless of inferred user intent. Use for
  actions that must never run in auto mode (e.g., `rm -rf /`, force-push to
  protected branches). Unlike `soft_deny`, hard-deny rules are not negotiable
  by the classifier. Documented in `09-advanced-features/README.md`.
- `parentSettingsBehavior` admin key (v2.1.133+, admin-tier) — controls how
  the SDK's `managedSettings` merges with parent-process settings.
  `"first-wins"` keeps existing precedence; `"merge"` deep-merges values.
  Documented in `09-advanced-features/README.md`.
- `Setup` hook event — initial environment setup (one-time per session); use
  to provision tooling or install deps. Brings the documented hook-events
  total from 28 to 29. Documented in `06-hooks/README.md`.
- `effort.level` field in hook input JSON (v2.1.133) — exposes the active
  effort level (`low`/`medium`/`high`/`xhigh`/`max`) to hooks. Documented in
  `06-hooks/README.md`.
- `CLAUDE_CODE_SESSION_ID` environment variable in Bash subprocesses
  (v2.1.132) — session UUID matching the `session_id` field in hook input
  JSON, for correlating bash logs with hook telemetry. Documented in
  `06-hooks/README.md`.
- `CLAUDE_EFFORT` environment variable in Bash subprocesses (v2.1.133) —
  active effort level, matching `effort.level` in hook input JSON. Documented
  in `06-hooks/README.md`.
- `sandbox.bwrapPath` and `sandbox.socatPath` settings (v2.1.133+, Linux/WSL)
  — point Claude Code at non-standard install locations for `bubblewrap` and
  `socat`. Default to `$PATH` lookup. Documented in
  `09-advanced-features/README.md`.
- `CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN` environment variable (v2.1.132).
  Documented in `09-advanced-features/README.md`.
- `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL` environment variable
  (v2.1.136) — re-enables the session-quality survey for organizations
  capturing OpenTelemetry data; off by default in OTEL deployments.
  Documented in `09-advanced-features/README.md`.

### Changed

- **Behavior change**: Plan mode now blocks all file writes unconditionally
  (v2.1.136), including when a matching `Edit(...)` rule exists in
  `permissions.allow`. Previously a permissive `Edit(...)` rule could let
  writes through in plan mode; that bypass is closed. Workflows that
  depended on the older behavior must exit plan mode (`Shift+Tab`) before
  editing. Documented in `09-advanced-features/README.md`.
- Plugin spaced slash commands (e.g., `/myplugin review`) now resolve to
  `/myplugin:review`. Plugin `skills` config entries no longer hide the
  default `skills/` directory — both are merged. Documented in
  `07-plugins/README.md`.
- MCP servers now persist across `/clear` (v2.1.132+). Documented in
  `05-mcp/README.md`.
- Subagents discover project, user, and plugin skills via the Skill tool
  (v2.1.133). Documented in `04-subagents/README.md`.
- `--permission-mode` is now honored when resuming plan-mode sessions
  (v2.1.132). Documented in `09-advanced-features/README.md`.
- `CronList` output now includes the qualifier(s) and the scheduled prompt
  body (v2.1.136), so you can audit what each cron will run without opening
  it. Documented in `09-advanced-features/README.md`.

### Fixed

- OAuth refresh-token concurrent-refresh race condition.
- INDEX.md count drift: Skills 28 → 16, Plugins 40 → 27, Hooks scripts
  8 → 9 (recounted from the markdown content tree). The new totals reflect a
  `.md`-only methodology that scopes counts to tutorial content rather than
  build artifacts and config.
- Stale source URLs in `CATALOG.md` (v2.1.118 → v2.1.138) and
  `claude_concepts_guide.md` (v2.1.117 → v2.1.138). Removed a duplicate
  legacy footer in the concepts guide.

### Notes for translation maintainers

The `vi/`, `zh/`, `uk/`, and `ja/` localized trees are community-maintained
and may lag the English source. Contributors syncing translations should diff
against the English files updated in this release.

## [v2.1.131] — 2026-05-06

### Synced to Claude Code v2.1.131

Bumps tutorial coverage from Claude Code v2.1.126 → v2.1.131 (May 6, 2026
release). Anthropic shipped v2.1.128, v2.1.129, and v2.1.131 since the last
sync; v2.1.127 and v2.1.130 were skipped and never released publicly.

### Added (English docs)

- `--plugin-url <url>` flag (v2.1.129) — fetches a plugin `.zip` archive from
  a URL for the current session. Repeatable. Documented in
  `07-plugins/README.md`.
- `CLAUDE_CODE_FORCE_SYNC_OUTPUT` env var (v2.1.129) — forces synchronous
  output for terminals where auto-detection misses (e.g., Emacs `eat`).
  Documented in `10-cli/README.md` and `09-advanced-features/README.md`.
- `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE` env var (v2.1.129) — enables
  background upgrades for Homebrew/WinGet installs (which normally do not
  auto-update). Documented in `10-cli/README.md` and
  `09-advanced-features/README.md`.
- `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY` env var (v2.1.129) — required
  to opt in to `/v1/models` gateway discovery (see Changed). Documented in
  `10-cli/README.md`.
- `disableRemoteControl` setting (v2.1.128) — admins can block
  `claude remote-control` and `/remote-control` via managed/policy scope.
  Documented in `09-advanced-features/README.md`.
- `--plugin-dir` accepts `.zip` archives (v2.1.128) — alongside directory
  inputs. Documented in `07-plugins/README.md`.
- `skillOverrides` accepts `"name-only"` and `"user-invocable-only"`
  (v2.1.129) — in addition to the previous `"on"`/`"off"`. Documented in
  `03-skills/README.md`.

### Changed

- **Behavior change**: Gateway `/v1/models` discovery is now **opt-in**
  (v2.1.129). Previously (v2.1.126), setting `ANTHROPIC_BASE_URL` automatically
  populated `/model` from the gateway's `/v1/models` endpoint. From v2.1.129,
  users must additionally set `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1`;
  without the env var, `/model` falls back to the built-in static list.
  Documented in `10-cli/README.md`.
- `/mcp` shows tool count per server and visually flags servers reporting 0
  tools (v2.1.128). Documented in `05-mcp/README.md`.
- Bare `/color` (no args) picks a random session color (v2.1.128); explicit
  `/color <name|hex>` continues to set a specific color. Documented in
  `01-slash-commands/README.md`.
- `--channels` flag now works with API-key (console) authentication
  (v2.1.128). Earlier releases required Pro/Max OAuth. Documented in
  `09-advanced-features/README.md`.
- Ctrl+R history picker defaults to **all prompts across all projects**
  (v2.1.129). Press Ctrl+S inside the picker to narrow scope to the current
  project. Documented in `09-advanced-features/README.md`.
- `/context` no longer dumps its ASCII visualization into the conversation
  (v2.1.129). The viz is shown in-UI only; no more ~1.6k token cost per
  invocation. Documented in `09-advanced-features/README.md`.
- Oversized images in drag-and-drop are auto-downscaled (v2.1.128) — earlier
  versions rejected images outright.

### Fixed

- VS Code extension activation on Windows (v2.1.131).
- Mantle endpoint authentication (v2.1.131).
- 1-hour prompt-cache TTL no longer truncated to 5 minutes (v2.1.129).
- Crash on stdin payloads larger than 10 MB (v2.1.128).

### Notes for translation maintainers

The `vi/`, `zh/`, `uk/`, and `ja/` localized trees are community-maintained
and may lag the English source. Contributors syncing translations should diff
against the English files updated in this release.

## [v2.1.126] — 2026-05-02

### Synced to Claude Code v2.1.126

Bumps tutorial coverage from Claude Code v2.1.119 → v2.1.126 (May 1, 2026 release).
v2.1.120 was rolled back on its first release day (2026-04-24) but re-released
successfully on 2026-04-28 with the originally-reported regressions fixed.
v2.1.124 and v2.1.125 were skipped by Anthropic and never released.

### Added (English docs)

- `claude project purge [path]` subcommand (v2.1.126) — deletes all Claude Code
  state for a project (transcripts, tasks, debug logs, file-edit history,
  prompt history, `~/.claude.json` entry). Supports `--dry-run`, `-y/--yes`,
  `-i/--interactive`, `--all`. Documented in `10-cli/README.md`.
- `claude plugin prune` subcommand (v2.1.121) — removes orphaned auto-installed
  plugin dependencies; `plugin uninstall --prune` cascades. Documented in
  `07-plugins/README.md`.
- `claude ultrareview [target]` subcommand (v2.1.120) — runs `/ultrareview`
  non-interactively from CI/scripts, prints findings to stdout, exits 0/1 on
  success/failure; supports `--json` and `--timeout <minutes>`. Documented in
  `10-cli/README.md`.
- `${CLAUDE_EFFORT}` placeholder available inside skill content (v2.1.120) —
  resolves to the current effort level. Documented in `03-skills/README.md`.
- `alwaysLoad` MCP server config option (v2.1.121) — when `true`, all tools
  from that server skip tool-search deferral. Documented in `05-mcp/README.md`.
- `PostToolUse.hookSpecificOutput.updatedToolOutput` now works for all tools
  (v2.1.121), previously MCP-only. Documented in `06-hooks/README.md`.
- `ANTHROPIC_BEDROCK_SERVICE_TIER` environment variable (v2.1.122) — selects
  Bedrock service tier (`default`, `flex`, `priority`). Documented in
  `10-cli/README.md` env-var table.
- `--dangerously-skip-permissions` extended-path coverage (v2.1.121, v2.1.126)
  — now bypasses prompts for writes to `.claude/skills/`, `.claude/agents/`,
  `.claude/commands/`, `.claude/`, `.git/`, `.vscode/`, shell config files.
  Catastrophic removal commands (`rm -rf /` etc.) still prompt. Documented in
  `09-advanced-features/README.md` permission-modes section.
- OAuth code paste fallback (v2.1.126) — `claude auth login` accepts the OAuth
  code pasted into the terminal when the browser callback can't reach
  localhost (WSL2, SSH, containers). Documented in `10-cli/README.md`.
- Type-to-filter `/skills` menu (v2.1.121). Documented in `03-skills/README.md`.
- `AI_AGENT` environment variable (v2.1.120) — set on subprocesses so `gh` can
  attribute traffic to Claude Code. Documented in `10-cli/README.md` env-var
  table.

### Changed

- `--from-pr` (v2.1.119) and `/resume` PR-URL search (v2.1.122) now both
  support GitHub, GitHub Enterprise, GitLab, and Bitbucket URLs.
- Windows: Git for Windows / Git Bash no longer required (v2.1.120) — Claude
  Code uses PowerShell as the shell tool when Git Bash is absent. From v2.1.126,
  PowerShell is the primary shell when the PowerShell tool is enabled. Detection
  extended to PowerShell 7 installed via Microsoft Store, MSI without PATH, or
  `.NET global tool`. Documented in `09-advanced-features/README.md` platform
  notes.
- `/model` picker now lists models from your gateway's `/v1/models` endpoint
  when `ANTHROPIC_BASE_URL` points at an Anthropic-compatible gateway
  (v2.1.126). Documented in `10-cli/README.md`.
- `--dangerously-skip-permissions` no longer prompts for writes to a much
  broader allowlist (see Added). Catastrophic removals still prompt.
- Image paste auto-downscale (v2.1.126) — images larger than 2000px are
  downscaled on paste; oversized images in history are auto-removed and the
  request retried. (Tutorial-relevant only as a safety/UX note.)

### Security

- Fixed `allowManagedDomainsOnly` / `allowManagedReadPathsOnly` being ignored
  when a higher-priority managed-settings source lacked a `sandbox` block
  (v2.1.126).

### Notes for translation maintainers

The `vi/`, `zh/`, `uk/`, and `ja/` localized trees are community-maintained
and may lag the English source. Contributors syncing translations should diff
against the English files updated in this release.

## [v2.4.0] — 2026-04-27

### Synced to Claude Code v2.1.119

Bumps tutorial coverage from Claude Code v2.1.112 → v2.1.119 (April 23, 2026 release).
v2.1.120 was published April 24, briefly rolled back the same day due to regressions,
and re-released on April 28 with fixes — it is now part of the normal release line.
A subsequent v2.1.126 (May 1, 2026) is the next stable target and is covered in the
v2.1.126 entry above.

### Added (English docs)

- Native binary packaging note (v2.1.113) — CLI now ships per-platform native binaries
- `bfs`/`ugrep` Glob/Grep substitution footnote on native macOS/Linux builds (v2.1.117)
- `mcp_tool` hook type with example (v2.1.118)
- `duration_ms` field on PostToolUse / PostToolUseFailure inputs (v2.1.119)
- `prUrlTemplate` setting (v2.1.119) and expanded `--from-pr` provider list (GitLab, Bitbucket)
- `cleanupPeriodDays` extended scope (checkpoints + tasks + shell-snapshots + backups, v2.1.117)
- Plugin marketplace enforcement on every lifecycle event (v2.1.117) and `hostPattern`/`pathPattern` regex (v2.1.119)
- New env vars: `DISABLE_UPDATES`, `CLAUDE_CODE_HIDE_CWD`, `CLAUDE_CODE_FORK_SUBAGENT`, `OTEL_LOG_TOOL_DETAILS`, `ENABLE_TOOL_SEARCH` Vertex opt-in
- New slash commands: `/btw`, `/theme` with custom themes
- `/usage` canonical command (merges `/cost` + `/stats`, v2.1.118)
- Forked subagents (`CLAUDE_CODE_FORK_SUBAGENT=1`, v2.1.117)
- Auto mode `"$defaults"` token (v2.1.118)
- `wslInheritsWindowsSettings` managed policy (v2.1.118)
- Vim visual / visual-line modes (v2.1.118)
- `claude install [version]` and `claude plugin tag` subcommands

### Changed

- Documentation host migrated: `docs.anthropic.com/en/docs/claude-code/*` → `code.claude.com/docs/en/*`
- Opus 4.7 effort levels: `xhigh` is now the Claude Code default since the 2026-04-16 launch; Opus 4.7 native context window confirmed at 1M (v2.1.117 fixed `/context` miscounting it as 200K)
- Default effort raised from `medium` to `high` for Pro/Max subscribers on Opus 4.6 / Sonnet 4.6 (v2.1.117)
- `STYLE_GUIDE.md` Source URL updated from Claude Apps article to `code.claude.com/docs/en/changelog`

### Deprecated (tracked, not removed)

- `includeCoAuthoredBy` setting → use `attribution.commit` / `attribution.pr`
- `voiceEnabled` setting → use `voice.enabled`

### Notes for translation maintainers

The `vi/`, `zh/`, and `uk/` localized trees are community-maintained and may lag the English source. Contributors syncing translations should diff against the English files updated in this release.

## v2.1.112 — 2026-04-16

### Highlights

- Sync all English tutorials with Claude Code v2.1.112 and the new Opus 4.7 model (`claude-opus-4-7`), including the new `xhigh` effort level (default on Opus 4.7, between `high` and `max`), two new built-in slash commands (`/ultrareview`, `/less-permission-prompts`), auto-mode no longer requiring `--enable-auto-mode` for Max subscribers on Opus 4.7, the PowerShell tool on Windows, the "Auto (match terminal)" theme, and plan files named after prompts. All 18 EN doc footers bumped to Claude Code v2.1.112. @Luong NGUYEN

### Features

- Add complete Ukrainian (uk) localization across all modules, root docs, examples, and references (039dde2) @Evgenij I

### Bug Fixes

- Correct pre-tool-check.sh hook protocol bugs (bce7cf8) @yarlinghe
- Change bad mermaid example to text block to pass CI (b8a7b1f) @Evgenij I
- Fix CP1251 encoding in Ukrainian claude_concepts_guide.md ToC (d970cc6) @Evgenij I
- Replace stub Ukrainian README with full translation, fix broken anchors (f6d73e2) @Evgenij I
- Correct Claude Code version to 2.1.97 across all footers (63a1416) @Luong NGUYEN
- Apply 2026-04-09 documentation accuracy updates (e015f39) @Luong NGUYEN

### Documentation

- Sync to Claude Code v2.1.112 (Opus 4.7, `xhigh` effort, `/ultrareview`, `/less-permission-prompts`, PowerShell tool, Auto-match-terminal theme) @Luong NGUYEN
- Sync to Claude Code v2.1.110 (TUI, push notifications, session recap) (15f0085) @Luong NGUYEN
- Sync to Claude Code v2.1.101 with `/team-onboarding`, `/ultraplan`, Monitor tool (2deba3a) @Luong NGUYEN
- Sync Vietnamese documentation with English source (561c6cb) @Thiên Toán
- Update Last Updated date and Claude Code version across all files (7f2e773) @Luong NGUYEN
- Add Ukrainian language link to language switcher (9c224ff) @Luong NGUYEN
- Remove contributors section (f07313d) @Luong NGUYEN
- Update GitHub metrics to 21,800+ stars, 2,585+ forks (4f55374) @Luong NGUYEN

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/v2.3.0...v2.1.112

---

## v2.3.0 — 2026-04-07

### Features

- build and publish EPUB artifacts per language (90e9c30) @Thiên Toán
- add missing pre-tool-check.sh hook to 06-hooks (b511ed1) @JiayuWang
- add Chinese translations in zh/ directory (89e89d4) @Luong NGUYEN
- Add performance-optimizer subagent and dependency-check hook (f53d080) @qk

### Bug Fixes

- Windows Git Bash compatibility + stdin JSON protocol (2cbb10c) @Luong NGUYEN
- correct autoCheckpoint config documentation in 08-checkpoints (749c79f) @JiayuWang
- embed SVG images instead of replacing with placeholders (1b16709) @Thiên Toán
- nested code fence rendering in memory README (ce24423) @Zhaoshan Duan
- apply review fixes dropped by squash merge (34259ca) @Luong NGUYEN
- make hook scripts compatible with Windows Git Bash and use stdin JSON protocol (107153d) @binyu li

### Documentation

- sync all tutorials with latest Claude Code docs (April 2026) (72d3b01) @Luong NGUYEN
- add Chinese language link to language switcher (6cbaa4d) @Luong NGUYEN
- add language switcher between English and Vietnamese (100c45e) @Luong NGUYEN
- add GitHub #1 Trending badge (0ca8c37) @Luong NGUYEN
- Introduce cc-context-stats for context zone monitoring (d41b335) @Luong NGUYEN
- Introduce luongnv89/skills collection and luongnv89/asm skill manager (7e3c0b6) @Luong NGUYEN
- Update README stats to reflect current GitHub metrics (5,900+ stars, 690+ forks) (5001525) @Luong NGUYEN
- Update README stats to reflect current GitHub metrics (3,900+ stars, 460+ forks) (9cb92d6) @Luong NGUYEN

### Refactoring

- replace Kroki HTTP dependency with local mmdc rendering (e76bbe4) @Luong NGUYEN
- shift quality checks to pre-commit, CI as 2nd pass (6d1e0ae) @Luong NGUYEN
- narrow auto-mode permissions baseline (2790fb2) @Luong NGUYEN
- Replace auto-adapt hook with one-time permissions setup script (995a5d6) @Luong NGUYEN

### Other

- shift-left quality gates — add mypy to pre-commit, fix CI failures (699fb39) @Luong NGUYEN
- Add Vietnamese (Tiếng Việt) Localization (a70777e) @Thiên Toán

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/v2.2.0...v2.3.0

---

## v2.2.0 — 2026-03-26

### Documentation

- Sync all tutorials and references with Claude Code v2.1.84 (f78c094) @luongnv89
  - Update slash commands to 55+ built-in + 5 bundled skills, mark 3 deprecated
  - Expand hook events from 18 to 25, add `agent` hook type (now 4 types)
  - Add Auto Mode, Channels, Voice Dictation to advanced features
  - Add `effort`, `shell` skill frontmatter fields; `initialPrompt`, `disallowedTools` agent fields
  - Add WebSocket MCP transport, elicitation, 2KB tool cap
  - Add plugin LSP support, `userConfig`, `${CLAUDE_PLUGIN_DATA}`
  - Update all reference docs (CATALOG, QUICK_REFERENCE, LEARNING-ROADMAP, INDEX)
- Rewrite README as landing-page-structured guide (32a0776) @luongnv89

### Bug Fixes

- Add missing cSpell words and README sections for CI compliance (93f9d51) @luongnv89
- Add `Sandboxing` to cSpell dictionary (b80ce6f) @luongnv89

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/v2.1.1...v2.2.0

---

## v2.1.1 — 2026-03-13

### Bug Fixes

- Remove dead marketplace link failing CI link checks (3fdf0d6) @luongnv89
- Add `sandboxed` and `pycache` to cSpell dictionary (dc64618) @luongnv89

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/v2.1.0...v2.1.1

---

## v2.1.0 — 2026-03-13

### Features

- Add adaptive learning path with self-assessment and lesson quiz skills (1ef46cd) @luongnv89
  - `/self-assessment` — interactive proficiency quiz across 10 feature areas with personalized learning path
  - `/lesson-quiz [lesson]` — per-lesson knowledge check with 8-10 targeted questions

### Bug Fixes

- Update broken URLs, deprecations, and outdated references (8fe4520) @luongnv89
- Fix broken links in resources and self-assessment skill (7a05863) @luongnv89
- Use tilde fences for nested code blocks in concepts guide (5f82719) @VikalpP
- Add missing words to cSpell dictionary (8df7572) @luongnv89

### Documentation

- Phase 5 QA — fix consistency, URLs, and terminology across docs (00bbe4c) @luongnv89
- Complete Phases 3-4 — new feature coverage and reference doc updates (132de29) @luongnv89
- Add MCPorter runtime to MCP context bloat section (ef52705) @luongnv89
- Add missing commands, features, and settings across 6 guides (4bc8f15) @luongnv89
- Add style guide based on existing repo conventions (84141d0) @luongnv89
- Add self-assessment row to guide comparison table (8fe0c96) @luongnv89
- Add VikalpP to contributors list for PR #7 (d5b4350) @luongnv89
- Add self-assessment and lesson-quiz skill references to README and roadmap (d5a6106) @luongnv89

### New Contributors

- @VikalpP made their first contribution in #7

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/v2.0.0...v2.1.0

---

## v2.0.0 — 2026-02-01

### Features

- Sync all documentation with Claude Code February 2026 features (487c96d)
  - Update 26 files across all 10 tutorial directories and 7 reference documents
  - Add documentation for **Auto Memory** — persistent learnings per project
  - Add documentation for **Remote Control**, **Web Sessions**, and **Desktop App**
  - Add documentation for **Agent Teams** (experimental multi-agent collaboration)
  - Add documentation for **MCP OAuth 2.0**, **Tool Search**, and **Claude.ai Connectors**
  - Add documentation for **Persistent Memory** and **Worktree Isolation** for subagents
  - Add documentation for **Background Subagents**, **Task List**, **Prompt Suggestions**
  - Add documentation for **Sandboxing** and **Managed Settings** (Enterprise)
  - Add documentation for **HTTP Hooks** and 7 new hook events
  - Add documentation for **Plugin Settings**, **LSP Servers**, and Marketplace updates
  - Add documentation for **Summarize from Checkpoint** rewind option
  - Document 17 new slash commands (`/fork`, `/desktop`, `/teleport`, `/tasks`, `/fast`, etc.)
  - Document new CLI flags (`--worktree`, `--from-pr`, `--remote`, `--teleport`, `--teammate-mode`, etc.)
  - Document new environment variables for auto memory, effort levels, agent teams, and more

### Design

- Redesign logo to compass-bracket mark with minimal palette (20779db)

### Bug Fixes / Corrections

- Update model names: Sonnet 4.5 → **Sonnet 4.6**, Opus 4.5 → **Opus 4.6**
- Fix permission mode names: replace fictional "Unrestricted/Confirm/Read-only" with actual `default`/`acceptEdits`/`plan`/`dontAsk`/`bypassPermissions`
- Fix hook events: remove fictional `PreCommit`/`PostCommit`/`PrePush`, add real events (`SubagentStart`, `WorktreeCreate`, `ConfigChange`, etc.)
- Fix CLI syntax: replace `claude-code --headless` with `claude -p` (print mode)
- Fix checkpoint commands: replace fictional `/checkpoint save/list/rewind/diff` with actual `Esc+Esc` / `/rewind` interface
- Fix session management: replace fictional `/session list/new/switch/save` with real `/resume`/`/rename`/`/fork`
- Fix plugin manifest format: migrate `plugin.yaml` → `.claude-plugin/plugin.json`
- Fix MCP config paths: `~/.claude/mcp.json` → `.mcp.json` (project) / `~/.claude.json` (user)
- Fix documentation URLs: `docs.claude.com` → `docs.anthropic.com`; remove fictional `plugins.claude.com`
- Remove fictional configuration fields across multiple files
- Update all "Last Updated" dates to February 2026

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/20779db...v2.0.0
