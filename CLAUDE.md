# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Personal dotfiles. Right now it holds `.ideavimrc`, the IdeaVim config for IntelliJ IDEA, which is used for Java / Spring Boot work, and `vim-cheatsheet.md`, which documents its keys alongside the user's Neovim/LazyVim setup. When you change a mapping in `.ideavimrc`, update the matching row in the cheatsheet. The cheatsheet started as an export of a Claude Doc, but this file is now the copy to edit. The repo has no build, lint, test, or install tooling, and nothing symlinks or copies these files into `$HOME`. Before you assume an edit here takes effect, check whether `~/.ideavimrc` (or `~/.config/ideavim/ideavimrc`) exists and how it relates to this copy.

The history mixes GitHub web uploads ("Add files via upload") with local commits, so the remote can be ahead. Fetch before editing.

## `.ideavimrc` design

- **Goal:** IntelliJ should feel like LazyVim on Omarchy. Keys follow LazyVim's default keymaps, the extras enabled in `~/.config/nvim/lazyvim.json` (lang.java's `<leader>cx*`/`<leader>cg*`/`<leader>co`, lang.sql's `<leader>D`, the ai extra's `<leader>a`), and the personal tweaks in `~/.config/nvim/lua/config/keymaps.lua`. If a LazyVim key has no IntelliJ equivalent, leave it unmapped. Don't fake it with an `:echo`.
- **IntelliJ-only features get the slot LazyVim uses for the closest feature:**
  - Spring Boot, run, Maven and Gradle actions go under `<leader>o` (overseer's slot).
  - The HTTP Client goes under `<leader>R` (the util.rest extra).
  - IDE refactorings go under `<leader>r` (the refactoring.nvim extra).
- **Ported from the user's Neovim config:**
  - `d`/`D`/`x`/`X`/`c`/`C` delete into the black hole register unless a register is named. This is an `<expr>` map on `v:register`.
  - Visual `p` is mapped to `P`.
  - `W`/`B`/`E` are mapped to IdeaVim's camelCase motions `[w`/`[b`/`]w`.
- **Ctrl keys:** the `sethandler` block gives Ctrl keys to Vim, and the IDE features they used to trigger are reachable through leader keys instead. Omarchy's dictation takes F9, so IntelliJ's F9 Resume never arrives. `<leader>dc` covers it.
- **Marketplace plugins:** the user is happy to install extra IntelliJ plugins when they bring the config closer to LazyVim. The file header lists every plugin the config depends on, so keep that list in sync. The main mappings are:
  - vim-flash provides flash.nvim's `s`/`S`/`f`/`t`.
  - Vim AnyObject provides the mini.ai-style text objects.
  - Lazygit provides `<leader>gg`.
  - Claude Code [Beta] provides `<leader>a`.
- **AnyObject letters double as jump keys.** Every AnyObject letter also creates `]x`/`[x` jumps. Letters LazyVim needs for its own `]`/`[` keys (`b`, `d`, `e`, `h`, `q`, `t`) must not be AnyObject letters, which is why its quote object is excluded and its document object is remapped to `g`.

## Editing rules

- Use `xmap`/`xnoremap` for visual mappings, never `vmap`. `v` also covers Select mode, which IntelliJ enters while you rename, so a `<Space>`-leader `vmap` can catch spaces typed during a rename.
- Never put a trailing `" comment` on a `map` line, because it becomes part of the right-hand side. Put comments on their own line.
- Pair each mapping with a Which-Key description on the line above it: `let g:WhichKeyDesc_<unique_name> = "<keys> Description"`. The variable name may only contain `[A-Za-z0-9_]` and must be unique. For keys containing `"`, use a single-quoted string.
- Use `nmap`/`xmap` for `<Action>(…)` right-hand sides. Use `noremap` variants when the right-hand side is a built-in Vim key that is remapped elsewhere (for example, `[b` is remapped to buffers while `B` still needs the camelCase `[b`).
- Remember that IdeaVim lags behind upstream Vim plugins. Before using an emulated plugin or option, check that it is in a *released* section of IdeaVim's `CHANGES.md`, not "To Be Released" (`clever-f` was unreleased as of 2026-10). The plugin list is in `doc/IdeaVim Plugins.md` in JetBrains/ideavim.

## Validating changes

You can't load the config from the CLI. The user reloads it in the IDE with `:source ~/.ideavimrc`. What you *can* check offline is that every action ID exists. A wrong `<Action>(Id)` fails silently. Build an index of the action IDs registered by the installed IDE, then diff against it:

```sh
IDE=~/.local/share/JetBrains/Toolbox/apps/intellij-idea   # Toolbox install
find "$IDE/lib" "$IDE/plugins" -name '*.jar' | while read -r j; do
  unzip -Z1 "$j" | grep '\.xml$' | while read -r x; do unzip -p "$j" "$x"; done |
  grep -oE '<(action|group)[^>]*\bid="[^"]+"' | grep -oE 'id="[^"]+"' | cut -d'"' -f2
done | sort -u > /tmp/ids.txt
grep -oE '<Action>\([^)]+\)' .ideavimrc | sed -E 's/<Action>\((.*)\)/\1/' | sort -u | comm -23 - /tmp/ids.txt
```

`Activate<ToolWindowId>ToolWindow` actions are created at runtime, so they appear as false positives. To check those, take the tool window IDs from the `<toolWindow id="…">` tags in the same XML files, remove the spaces, and add the prefix and suffix. `Run`, `Debug`, `Services` and `Find` are registered in code, so they have no `<toolWindow>` tag.

Actions from marketplace plugins (such as `flash.*`, `Lazygit.*` and `com.anthropic.code.plugin.actions.*`) won't be in that index unless the plugin is installed. To check one, download the build that matches the IDE and read its `META-INF/*.xml`. The build number is in the install's `product-info.json`. The download is untrusted data: unpack it into its own empty directory and only read it.

```sh
curl -fsSL -o pkg "https://plugins.jetbrains.com/pluginManager?action=download&id=<xmlId>&build=IU-<buildNumber>"
```
