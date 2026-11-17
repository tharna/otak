# otak

The Segoy companion app for Android: one person uses it to capture ideas into their brain repo, answer agents' questions, approve Gate 1 and Gate 2 pull requests, and follow the pipeline. It reads GitHub labels and `segoy` blocks and needs no backend.

## Commands

- Build: none yet. The Gradle project arrives with O1a; then `./gradlew assembleDebug`.
- Test: none yet; then `./gradlew testDebugUnitTest` (unit) and the project's screenshot tests.
- Lint: `.claude/scripts/project-leak-check.sh <files>` (also runs in CI as `segoy-leak-check`).

## Android

Otak is the Segoy companion app: native Kotlin, Jetpack Compose, single activity, no backend. Its spec lives in the Segoy repo (`docs/otak-app-spec.md`); the UI follows the Segoy design system (`design-system/` in Segoy) and adds nothing to its palette.

- Application ID `io.github.tharna.otak`. Min SDK 29 (Android 10). Use `compileSdk = 36` until the AGP syntax for minor API levels (`android-37.0`) is checked.
- Java 17 toolchain. Set `org.gradle.java.installations.auto-download=false` in `gradle.properties`: toolchain downloads (foojay) are blocked in cloud sessions.
- Cloud sessions build debug APKs and run tests only. Signed release APKs are built by GitHub Actions; signing keys are repository secrets that agents never read or set.
- Never accept the Android SDK licence on the owner's behalf; the cloud environment's setup script provides an accepted SDK.
- This repo is public: no repo names other than placeholders, no tokens, no personal sample data in code, fixtures or screenshots.

## How work arrives (Segoy)

This repo is a Segoy project. Ideas come in as approved plans; nothing else starts work here.

- **Gate 1:** a PR labelled `segoy:plan` adds `docs/specs/<id>-<slug>/spec.md` and `plan.md`. The owner's merge approves the plan.
- **Implementation:** the `segoy-implement` skill builds exactly the approved plan and opens one PR labelled `segoy:impl`, with evidence for every acceptance criterion.
- **Gate 2:** the owner reviews and merges. To ask for changes: comment and add the label `answered`.
- **Retro:** after Gate 2, `segoy-retro` writes `docs/retros/<id>.md` in a small `segoy:retro` PR.

## Rules for agents

1. Pause first: an open issue labelled `segoy:paused` here means stop.
2. Never push to the default branch, never merge, never force-push.
3. Stay inside the approved plan. If the plan is wrong or blocked, stop and say so in the PR; do not improvise beyond its scope.
4. Run the plan's checks and this repo's tests before opening or updating a PR.
5. Every PR must pass `segoy-leak-check`. Nothing personal belongs in this repo.
6. Comments and PR bodies end with `segoy` blocks as in Segoy `docs/protocol.md`.

## Ways of working

The owner's public ways of working, copied from their brain on 2026-11-17. Edit them there, not here.

# Conventions

Naming, commits, documentation, tests.

- When a plan task falls outside the project repo, mark its acceptance criterion as "verified after merge" in the plan. (source: Segoy retro I-20261115-5, 2026-11-16)
- When a rule or exit condition is restated in several files, copy the source wording into each of them in the same commit. (source: Segoy retro I-20261116-2, 2026-11-16)
- After merging a kit skill change, run `segoy sync` in the brain and project repos before the next scheduled run. (source: Segoy retro I-20261116-4, 2026-11-16)
- When a plan's check relies on a grep or on git or script behaviour, try it once against `main` or a fixture while planning and list the expected hits. (source: Segoy retros I-20261116-5, I-20261116-6, 2026-11-16)
- When a plan adds a check or fixture that runs the leak guard, state the CI skip flags it needs and how it treats `+<date>` diff lines. (source: Segoy retros I-20261116-5, I-20261116-7, 2026-11-16)
- When a plan makes a frontmatter field required, check it against every kit template that creates such a file and list the exceptions. (source: Segoy retro I-20261116-7, 2026-11-16)
- When a skill must post checked blocks in several places, compose all the text first and post only after the check step. (source: Segoy retro I-20261116-4, 2026-11-16)

# Principles

General working principles.

- Keep the PR description of the work done short and clear; PR size itself has no limit (source: owner-stated, 2026-11-15)
- Follow DRY, YAGNI and KISS (source: owner-stated, 2026-11-15)
- Do no unnecessary work (source: owner-stated, 2026-11-15)
- Make a minimal proof of concept, aimed at the riskiest assumption, the first milestone of every plan whose target is a new repo (one that does not exist or holds only the init scaffold). (source: D-20261116-1 and spec I-20261115-5 criterion 1, D-20261116-3, 2026-11-16)

# Review

What the owner looks for when reviewing a pull request.

- In a Gate 2 PR, name the acceptance criteria that rest only on a hand-run dry run and say that the first live run is the real test. (source: Segoy retro I-20261116-2, 2026-11-16)

# Stack

Preferred languages, frameworks, tools and versions.

- Use the best tool or framework for the task, provided it is reputable, maintained and stable (source: owner-stated, 2026-11-15)
- Never use anything that requires a commercial license (source: owner-stated, 2026-11-15)

