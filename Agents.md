This file is the single source of truth for contributors (Codex + humans). It supersedes split/older variants and always provides a complete overview.

Update Policy (mandatory)

Always keep these files up to date and in sync with reality:
AGENTS.md (this file)
ROADMAP.md
CHANGELOG.md
Immediately after a patch is fully applied (no waiting for external build/test):
Update CHANGELOG.md with a brief note of changes (or move items from roadmap to changelog).
Update ROADMAP.md (remove completed items; keep near-/mid-term actionable items only).
If the change is substantial (new modules/flows/features or architectural impact), also update AGENTS.md so a one-stop read remains accurate.
Approvals: Routine repository/documentation updates are applied directly without prior diff approval. Approvals are only required for privileged/irreversible operations or external authentication.
Codex – Operating Rules (override)
Where the runtime environment imposes sandbox/approval limits, Codex follows the intent and asks for minimal one‑shot approval when required.
Single source of truth: AGENTS.md is the canonical source for workflow, architecture, and dependencies. All other documents (including ARCHITECTURE_OVERVIEW.md, ROADMAP.md, CHANGELOG.md) are derived/synced from here. In case of discrepancies, AGENTS.md prevails.
Full context gathering: For a complete overview, Codex also reads ROADMAP.md, CHANGELOG.md, ARCHITECTURE_OVERVIEW.md, and the latest commits/PRs to understand the current state before making changes.
Auto documentation upkeep: Immediately after patches, Codex updates the full documentation set (AGENTS.md, ROADMAP.md, CHANGELOG.md, ARCHITECTURE_OVERVIEW.md) and pushes to master. If the environment blocks direct writes or pushes, Codex prepares diffs and requests the smallest possible approval to finalize.
Deep dependency awareness: When patching, Codex reads all relevant modules in appropriately sized batches and considers all dependent modules. ARCHITECTURE_OVERVIEW.md is maintained as a detailed, human‑friendly derivative; if it disagrees with this file, this file wins.
Cascading fixes allowed: If additional modules must change to keep the system consistent after a patch, Codex proceeds to implement those changes directly under these rules.
End‑to‑end execution: When the user requests a change/fix/implementation, Codex performs it end‑to‑end (no TODOs/placeholders). For major changes requiring iterative passes over the same files, Codex proceeds autonomously without waiting for intermediate applies, unless sandbox constraints force an approval.
Minimize approvals: Routine repo/Documentation changes are applied directly. Approvals are limited to privileged ops, irreversible deletions, or external authentication.
Pragmatic alternatives: If a request is technically not feasible, Codex proposes the best alternative solution and requests approval where appropriate.
Respectful scope: Codex does not change/trim/expand modules or files without instruction, except where necessary to uphold these rules or maintain architectural integrity. Existing flows (EPG/Xtream, player paths, list/detail) must be preserved unless requested.
Ongoing hygiene: Codex periodically tidies the repo, highlights obsolete files/code to the user, and removes uncritical leftovers (e.g., stale *.old files). Never touch .gradle/, .idea/, or app/build/ artifacts, and avoid dependency upgrades unless fixing builds.
Cross‑platform builds: Codex uses Linux/WSL for builds/tests via Gradle wrapper while keeping settings compatible with Windows. Ensure no corruption of Windows‑side project files.
WSL build files: Projektstamm enthält Linux‑spezifische Ordner für Build/Tests: .wsl-android-sdk, .wsl-gradle, .wsl-java-17. Codex verwendet diese Ordner unter WSL; Windows‑seitige Einstellungen bleiben kompatibel.
Tooling upgrades: If Codex needs additional tools or configuration to work better, it informs the user and, where possible, sets them up itself; otherwise it provides clear, copy‑pastable step‑by‑step commands for the user to establish the optimal environment.
Quick Build & Test


The canonical, continuously updated source is AGENTS.md (this file).
ARCHITECTURE_OVERVIEW.md is a detailed, human‑friendly derivative of this file. If discrepancies occur, this file prevails. Keep ARCHITECTURE_OVERVIEW.md updated whenever new modules/features are added.
Short bullet summary (current highlights)
