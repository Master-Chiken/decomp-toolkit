# Boonta Reborn 88 — decomp-toolkit evaluation

Status: evaluation note only. This fork has not been modified to change decomp-toolkit's behavior, and no integration with Boonta Reborn 88 has been tested yet.

## Decision

Keep decomp-toolkit as a separate developer tool. Do not copy its source tree or binary into the game repository, and do not make it a runtime dependency of the GameCube/Wii game.

The upstream project's main purpose is matching decompilation of existing GameCube/Wii binaries. Boonta Reborn 88's current clean-room demo is a native homebrew build, not a matching decompilation project. Therefore, most of the matching/decompilation workflow is not directly applicable.

## Initial upstream metadata check

The upstream `Cargo.toml` currently declares version `1.8.4` and license expression `MIT OR Apache-2.0`. A root-level file named `LICENSE` was not found by the repository file lookup, so verify the actual license text files and dependency notices before redistributing binaries or modified source. Do not treat this metadata check as a full legal review.

## Potentially useful, limited scope

1. **Inspect our own build artifacts.** Evaluate `dtk dol info` against a Boonta Reborn 88 DOL produced by the existing native build. Compare its section/entry-point report with the current artifact validator.
2. **Inspect ELF artifacts.** Evaluate the documented ELF commands only where they apply to the exact ELF produced by our toolchain; do not assume every command supports every ELF format.
3. **Optional diagnostics, not a replacement for tests.** A dtk report can add a second view of binary structure, but it does not prove that the game is playable, that controls work, or that a three-lap race can finish.
4. **Use reverse engineering only with lawful inputs.** Do not commit commercial game disc images, ROMs, extracted proprietary assets, keys, or copyrighted payloads to either public repository. Do not use copyrighted game data as a source for redistribution.

## Free-only and repository safety rules

- No paid service, billing, card, or paid fallback.
- Do not create a branch just for this evaluation unless a later code change genuinely needs an isolated review branch.
- Never delete existing branches or rewrite their history.
- In `Master-Chiken/boonta-reborn-88`, preserve the current branch cap of 105 and keep `main` protected; use the existing intake/PR workflow for any eventual game-repository change.
- Pin a reviewed release/version before adding any automated dtk check; avoid downloading an unpinned moving target in CI.
- Check the exact upstream license and dependency licenses before redistributing modified code or binaries. The presence of a fork is not itself permission to change or relicense upstream material.

## Acceptance test before integration

- [x] Read upstream `Cargo.toml` and record its declared version/license expression (not a complete license review).
- [ ] Verify the actual license text files and relevant dependency-license obligations.
- [ ] Run a pinned dtk version against a Boonta Reborn 88 build artifact only.
- [ ] Record command, tool version, exit code, and output.
- [ ] Confirm whether the result adds a check not already covered by `gamecube-demo/tools/validate_artifacts.py`.
- [ ] Keep the tool optional if it provides only duplicate information.
- [ ] Do not merge any new workflow into the game repository until it is tested and reviewed.

## References

- Fork: https://github.com/Master-Chiken/decomp-toolkit
- Upstream: https://github.com/encounter/decomp-toolkit
- Game project: https://github.com/Master-Chiken/boonta-reborn-88
- Current game build status: https://github.com/Master-Chiken/boonta-reborn-88/blob/main/docs/GAMECUBE_FIRST_RENDER_STATUS.md
