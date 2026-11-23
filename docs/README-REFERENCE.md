# README Reference

This page holds details that are useful after the main README's quick start, without interrupting the main reader path.
#### Tools the installer does not wire, and what to do instead

Each of these was checked against its own documentation. Where a tool can block a command but its format could not be confirmed, you get the manual step rather than a guess.

| Tool | What to do |
|---|---|
| Kiro CLI | Kiro sends the event as JSON on stdin and blocks the tool when a `PreToolUse` command hook exits non-zero, and the guard reads Kiro's `shell`, `execute_bash`, `execute_pwsh` and `execute_cmd` payloads. The installer does not wire it yet. After `install.sh`, add `.kiro/hooks/app-store-compliance.json` to the project with `{"version":"v1","hooks":[{"name":"app-store-compliance","trigger":"PreToolUse","matcher":"^(shell\|execute_bash\|execute_pwsh\|execute_cmd)$","action":{"type":"command","command":"bash ~/.claude/hooks/app-store-compliance-guard.sh"}}]}`. Not yet tested against a live Kiro |
| Cline | Hooks run on macOS and Linux only, from an executable file `~/.cline/hooks/PreToolUse` that answers `{"cancel":true}`. Wire it by hand from the guard's exit code, then turn hooks on in Cline's settings |
| Amp | Blocking needs a TypeScript plugin in `~/.config/amp/plugins/`. Not installed automatically yet. Run the guard by hand before a submit |
| Goose, Crush | Both can block, and their file layout could not be confirmed. Run the guard by hand before a submit |
| Roo Code, Continue | No hook, only a deny list. Add the submit commands (`fastlane`, `xcrun altool`, `eas submit`, `gradle bundleRelease`) to Roo's `deniedCommands` or to the exclude list in Continue's `permissions.yaml` |
| Aider, Copilot for Xcode | No way to run a check before a command. Run the guard by hand, or as a git pre-push hook, before every submit |

It never touches your GitHub account or your credentials. Undo it with the same command and `uninstall`, for example `install.sh --tool gemini uninstall`. The shared skill stays until the last tool using it is removed.

## What is inside

| Path | What it holds |
|---|---|
| `docs/WHAT-THE-SCAN-PROVES.md` | What the scan can prove and what only a person can, the readiness file, how current the rules are, Apple and Play coverage side by side, an honest take on 4.3 spam, reported rejections mapped to checks, and what the scripts do on your machine |
| `docs/FAQ.md` | Every question about the tool from the X replies, answered in full. Google Play and Mac coverage, install safety, guideline numbers on findings, the hook and fastlane, update speed, review times, and the licence |
| `docs/APPLE.md` | Apple rejection map, sections 1 to 5, every guideline with the trigger and the fix, plus the 2026 age rating and AI disclosure changes |
| `docs/GOOGLE-PLAY.md` | Google Play rejection map across every policy, plus the four level enforcement ladder from rejection to account termination |
| `docs/ADVANCED-2026.md` | The modern upload time layer (privacy manifests, export compliance), payments and DMA depth, the full legal layer (GDPR, EU AI Act, DSA, COPPA), gambling depth, AI content policy, and Android specifics |
| `docs/EU-REGULATORY-2026.md` | The EU legal hard rules with dated sources. the EU AI Act (Article 50 transparency by 2 August 2026, Article 4 literacy, Article 5 prohibitions, penalties), the Digital Markets Act and the Core Technology Fee, DSA trader status, the European Accessibility Act and EN 301 549, and the Apple 2025 and 2026 platform changes |
| `docs/GLOBAL-REGULATORY-2026.md` | The USA and other-global legal hard rules with dated sources. US COPPA and the state app-store age laws, the external-link rules, plus the UK, Australia, Brazil, and other jurisdictions, and what Apple tells developers to do per region |
| `docs/PLATFORM-MECHANICS-2026.md` | The platform-mechanics and newer-policy hard rules with dated sources. macOS notarization, Guideline 4.2 and 4.3 with the June 2026 saturation tightening, reader-app entitlement, France ANSSI encryption, visionOS and watchOS and tvOS specifics, plus Android developer verification, Foreground Service types, Play Integrity, Play Billing v8, target API, Health Connect, and the cross-cutting CSAM, UGC, accessibility, sanctions, and PCI items |
| `docs/CROSS-PLATFORM-FRAMEWORKS.md` | What the guard covers for Flutter, React Native, Expo, Capacitor, Ionic, Cordova, Unity, .NET MAUI, Tauri mobile, and Kotlin Multiplatform apps, and why it scans the built artifact surface rather than framework source |
| `docs/BY-APP-TYPE.md` | The rejection map routed by app type. Universal, subscriptions, social, kids, health, games, macOS, AI, crypto and finance, VPN |
| `docs/COMPETITIVE-GAP-ANALYSIS.md` | A survey of the other open source compliance repositories, what each publishes and why, the gaps they surfaced, and what was folded in here |
| `docs/REGULATORY-GAP-REPORT-2026.md` | Global and regional regulatory compliance gap analysis prepared by the Senior Compliance Officer, evaluating modern and upcoming frameworks |
| `docs/OPEN-SOURCE-PATTERNS.md` | What the community already codified. The fastlane precheck metadata rule set, the Android Play Policy Insights and security lints, community rejection repositories, and the Google Play pre-launch report, folded in |
| `docs/OTHER-STORES.md` | Huawei AppGallery, the Chinese stores, Samsung, Amazon, Microsoft, and RuStore, plus the cross store patterns worth adopting |
| `docs/GAMBLING-MATRIX.md` | Per country loot box and real money gambling rules, for games that ship worldwide |
| `docs/MISTAKE-PATTERNS.md` | The eight root cause patterns, the top mistakes, and the appeal playbook |
| `docs/PRE-SUBMISSION-CHECKLIST.md` | Exhaustive pre submission checklists for both stores, every item a verifiable check |
| `docs/FLEET-SWEEP.md` | Run the guard across every app you ship for one release verdict, with expiring per-occurrence waivers for reviewed false positives and a GitHub code-scanning export |
| `docs/RULING-REGISTER.md` | Release requirements a code scan cannot see, each with its legal standing, how it is proven, and a quoted source, built into Fleet Sweep and scored by `scripts/eval-scorecard.py` |
| `docs/AI-POLICY-MIGRATION.md` | Platform-specific generative AI policy tracking, generated by `scripts/monitor-ai-policy.py` |
| `docs/ANDROID-POLICY-MIGRATION.md` | Android and Google Play requirements tracking, generated by `scripts/monitor-android.py` |
| `docs/MOBILE-PRIVACY-MONITOR-2026.md` | Mobile and web privacy compliance monitoring output |
| `docs/PRIVACY-POLICY-MIGRATION.md` | Simulated privacy-policy migration output, generated by `scripts/monitor-privacy.py --simulate`, not live announcements |
| `docs/SECURITY-POLICY-MIGRATION.md` | Simulated security-policy migration output, generated by `scripts/monitor-security.py --simulate`, not live announcements |
| `docs/REGULATORY-TIMELINE.md` | Chronological global regulatory timeline, generated by `scripts/generate-timeline.py` from the deadline database |
| `docs/GAP-ANALYSIS-2026-09.md` | September 2026 gap analysis. what changed on Apple, Google Play, and in law since the previous sweep, what developers report in public, and what the playbook added or corrected |
| `docs/MOBILE-SECURITY-2026.md` | Mobile security requirements playbook (secure storage, backup exposure, deep link hijacking, and the checks each maps to) |
| `AGENTS.md` | Release review guidelines for AI agents, plus the source trust hierarchy and verification rules every monitor script follows before citing a claim as fact. A monitor's corroboration is keyword based, so read the cited official source before acting |
| `data/rejection-patterns.json` | Machine readable taxonomy of rejection patterns with detection signals and fixes. Drives the guard |
| `data/detection-recipes.json` | The detection command for each rejection pattern a code scan can see (107 of 114), generated into `references/`. The other seven are store listing and account checks done by hand |
| `data/regulatory-deadlines.json` | Global regulatory deadline database (jurisdiction, law, effective/grace/mandatory/enforcement dates), read by `scripts/deadline-checker.py` |
| `agent-os/skill/SKILL.md` | An agent skill that runs a full pre submission compliance audit |
| `agent-os/hooks/app-store-compliance-guard.sh` | The tested pre submission guard, usable standalone or as an agent hook |
| `scripts/verify-citations.py` | Verifies every cited URL resolves to real content, catching soft-404 pages that return HTTP 200 |
| `scripts/monitor-privacy.py` | Monitors mobile and web privacy requirements across Apple, Google, and EU sources |
| `scripts/monitor-security.py` | Monitors 17 mobile security requirements, matching real API symbols in your code |
| `scripts/generate-timeline.py` | Compiles a chronological regulatory timeline from the deadline database |
| `scripts/monitor.py` | Monitors 25 Apple developer requirements tracks, maps announcements to tracks, identifies affected files, generates migration tasks, estimates release impact, and drafts pull requests |
| `scripts/*-test.sh`, `scripts/test-*.py` | One test suite per script (guard, monitors, audits, deadline checker, timeline, citation verifier, metadata audit). CI runs every one of them on every push to main and on every pull request |
| `scripts/monitor-regulatory.py` | The Regulatory Intelligence Agent. tracks EU/UK/US/CA/AU/SG regulatory developments through a source trust hierarchy classifier |
| `scripts/monitor-android.py` | Android and Google Play requirements compliance monitor |
| `scripts/monitor-ai-policy.py` | Platform-specific generative AI policy monitor (Apple and Google) |
| `scripts/deadline-checker.py` | Prints every regulatory deadline inside a rolling 90-day window, timezone-aware. Runs automatically as part of the guard |
| `scripts/release-audit.py` | Release readiness compliance audit engine, writes `RELEASE-READINESS-REPORT.md` into the audited project. Runs the guard and the listing audit, and `--metadata <dir>` points the listing audit at a pulled metadata folder. Exit 0 is clear. Exit 2 is blocked on a critical or high finding. Exit 3 is incomplete, a scan failed or did not check the app |
| `scripts/accessibility-audit.py` | Continuous accessibility compliance audit (VoiceOver, Dynamic Type, Reduce Motion, contrast, TalkBack, touch targets). Runnable standalone, deliberately not wired into the guard hook |
| `scripts/validate.py` | Validates that `data/rejection-patterns.json`, `data/detection-recipes.json`, and `data/regulatory-deadlines.json` stay internally consistent. Runs in CI |
| `scripts/generate-references.py` | Regenerates `references/` from the pattern and recipe data, so the reference tree never drifts from the taxonomy |
| `scripts/metadata-audit.py` | Audits the live store listing (name, subtitle, keywords, description, URLs) against the metadata rejection rules, with a propose and re validate loop. Every locale folder is audited on its own, filled from a fastlane `default/` folder and parent folders like fastlane does, and each finding names its locale. `--platform google` applies the Google Play limits and rules, and a fastlane `android` folder gets them automatically. It checks that every URL is a real http or https address, that an Apple listing has a support URL, and that a Google listing has a contact email. A large share of rejections live in the listing, not the code |
| `scripts/install.sh` | One command install for Claude Code, and with `--tool` for Codex, Gemini CLI, OpenCode, Cursor, Copilot, Grok, Qwen Code, Kimi Code, Factory Droid, Auggie and Windsurf. `doctor` proves the hook blocks a submit command, `uninstall` removes it, `--version` prints the version |
| `scripts/install_harness.py` | The part of the install for the tools other than Claude Code. Merges each tool's own config file and writes the OpenCode plugin |
| `scripts/pull-metadata.sh` | Pulls the live App Store Connect listing into a metadata directory via the asc CLI, with the Play API path documented |
| `references/` | A structured, AI loadable reference tree. Rules by category (metadata, privacy, payments, design, performance, entitlements, safety, Android) and guidelines by app type, each with a detection command, generated from the taxonomy. Load the slices that match the task |
| `templates/REVIEW-NOTES-TEMPLATE.md` | A fill in the blanks App Store review notes template, the six sections that clear most 2.1 rejections |
| `docs/CREDITS.md` | Attribution for every open source repository and tool this playbook learned from |
| `LICENSE`, `NOTICE` | The OpenRoots Agent License 2.3 pointer and the short notice. See the License section below |
