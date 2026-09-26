# Neovim revival

Status: **REACTIVATING**

This repository is the personal Neovim configuration/developer-environment track.
It is cross-project developer tooling rather than a child of the Home Assistant
stack. The HA Stack may consume this environment for C/C++, shell, YAML, and
configuration work, but does not own it.

## Baseline

- Configuration base: `nvim-lua/kickstart.nvim`
- Revival upstream baseline: `80743df53d8f7058fc5b60e41f1081d11df9c880`
  (2026-09-14)
- Target Neovim stable release at revival: **v0.12.5**
- GitHub Actions remain intentionally absent from this fork. Validation is local
  unless CI is deliberately reintroduced later.

## Two preserved work tracks

1. **Personal configuration**
   - Keep this fork current enough to support the current stable Neovim release.
   - Make user-specific changes in small reviewable commits.
   - Track a plugin lockfile after the first validated local plugin sync.
   - Keep project-specific behavior modular instead of baking HA Stack assumptions
     into the base editor configuration.

2. **Neovim source-build/development**
   - Preserve the older Steam Deck source-build work as a related track rather
     than confusing it with the configuration fork.
   - The previous build-state issue involved stale CMake state and a misspelled
     build type. Current Neovim documentation uses `RelWithDebInfo`.
   - Recovery sequence from the Neovim source root:

```sh
make distclean
make CMAKE_BUILD_TYPE=RelWithDebInfo
./build/bin/nvim --version
```

   - If changing build type or install prefix again, clear `build/` first;
     `make distclean` also clears bundled dependency state.

## First-pass validation

Do this on the target Arch/Steam Deck machine before merging the revival branch:

1. Confirm `nvim --version` and decide whether the packaged binary or the local
   source build is the active executable.
2. Launch the revived configuration in parallel with any existing config using
   `NVIM_APPNAME` rather than overwriting a working setup.
3. Run `:checkhealth`.
4. Let Lazy install/synchronize plugins and record any failures.
5. Open representative C++, CMake, shell, YAML, Markdown, and JSON files.
6. Verify LSP, completion, Treesitter, Telescope, Git signs, clipboard, and
   terminal behavior.
7. Generate and commit `lazy-lock.json` after the plugin set is known-good.
8. Only then merge the revival branch into `master`.

## Relationship to HA Stack

No Neovim/Nvim-owned files or historical matching commits were found in the
current `tsrnc2/librem5-ha-stack` repository during the revival audit.
Treat HA Stack as a consumer of this editor environment, not as its canonical
parent. If an HA-specific module is added later, keep it optional and isolated.
