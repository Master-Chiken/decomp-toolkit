# Boonta Reborn 88 — decomp-toolkit evaluation

Status: evaluation note only. This fork has not been modified to change decomp-toolkit's behavior, and no integration with Boonta Reborn 88 has been tested yet.

## Decision

Keep decomp-toolkit as a separate developer tool. Do not copy its source tree or binary into the game repository, and do not make it a runtime dependency of the GameCube/Wii game.

The upstream project's main purpose is matching decompilation of existing GameCube/Wii binaries. Boonta Reborn 88's current clean-room demo is a native homebrew build, not a matching decompilation project. Therefore, most of the matching/decompilation workflow is not directly applicable.

## Pinned evaluation version

- Upstream release: `v1.8.4`, published 2026-09-09.
- Upstream release page: https://github.com/encounter/decomp-toolkit/releases/tag/v1.8.4
- Upstream `Cargo.toml`: version `1.8.4`, license expression `MIT OR Apache-2.0`.
- macOS Apple Silicon binary: `dtk-macos-arm64`, SHA-256 `5ba1fd76adf937847277a1ebe2238b698df0020c3dbbde23ecb126f00fb10353`.
- macOS Intel binary: `dtk-macos-x86_64`, SHA-256 `84bcab745a070bb970cf4bc0d69ddce9f6a58d5de257999ccc8adfa1567d17c9`.
- The upstream repository includes `LICENSE-MIT` and `LICENSE-APACHE`; review these and dependency-license obligations before redistributing modified code or binaries.

## Reproducible local smoke test (macOS)

This is optional and needs no paid service. It downloads a prebuilt upstream tool and a build artifact of **our own game only**. The latest known game build artifact is from main commit `7618fd118ecb53d486ffcb471b0aac3aa475746c`, workflow run [38799534736](https://github.com/Master-Chiken/boonta-reborn-88/actions/runs/38799534736). GitHub artifacts expire, so if this artifact has expired, use a newer successful run.

1. In Terminal, run `uname -m`. Choose `dtk-macos-arm64` for `arm64`, or `dtk-macos-x86_64` for `x86_64`.
2. Download the matching binary from the pinned [v1.8.4 release](https://github.com/encounter/decomp-toolkit/releases/tag/v1.8.4). Verify its SHA-256 against the value above before running it:
   - `shasum -a 256 dtk-macos-arm64`
   - or `shasum -a 256 dtk-macos-x86_64`
3. Make the binary executable and check its help:
   ```sh
   chmod +x ./dtk-macos-*
   ./dtk-macos-arm64 --help
   ```
   Use the actual filename you downloaded. Do not run both commands as if both files were present.
4. Open the game workflow run linked above, download the artifact named `boonta-reborn-gamecube-first-render`, and extract it to a temporary directory. Do not upload or commit the artifact back to either repository.
5. Run `dtk dol info /path/to/boonta_gc_demo.dol` using the actual path and the filename shown inside the artifact. Save the output and exit code for comparison with the existing validator.

If the binary is blocked by macOS or the checksum differs, stop and report that result; do not bypass security controls or use a different unpinned binary.

## Potentially useful, limited scope

1. **Inspect our own build artifacts.** Evaluate `dtk dol info` against a Boonta Reborn 88 DOL produced by the existing native build. Compare its section/entry-point report with the current artifact validator.
2. **Inspect ELF artifacts only if useful.** Evaluate documented ELF commands only where they apply to the exact ELF produced by our toolchain; do not assume every command supports every ELF format.
3. **Optional diagnostics, not a replacement for tests.** A dtk report can add a second view of binary structure, but it does not prove that the game is playable, that controls work, or that a three-lap race can finish.
4. **Use reverse engineering only with lawful inputs.** Do not commit commercial game disc images, ROMs, extracted proprietary assets, keys, or copyrighted payloads to either public repository. Do not use copyrighted game data as a source for redistribution.

## Free-only and repository safety rules

- No paid service, billing, card, or paid fallback.
- Do not create a branch just for this evaluation unless a later code change genuinely needs an isolated review branch.
- Never delete existing branches or rewrite their history.
- In `Master-Chiken/boonta-reborn-88`, preserve the current branch cap of 105 and keep `main` protected; use the existing intake/PR workflow for any eventual game-repository change.
- Pin a reviewed release/version before adding any automated dtk check; avoid downloading an unpinned moving target in CI.
- Check the exact upstream license and dependency licenses before redistributing code or binaries. The presence of a fork is not itself permission to change or relicense upstream material.

## Acceptance test before integration

- [x] Read upstream `Cargo.toml` and record its declared version/license expression (not a complete legal review).
- [x] Identify pinned v1.8.4 release assets and their published SHA-256 digests.
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
