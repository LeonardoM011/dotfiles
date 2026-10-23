# Vim Cheatsheet (LazyVim + IdeaVim)

Oct 22, 2026 · @Leonardo

Core Vim works the same in Neovim (LazyVim on Omarchy) and IntelliJ (`~/Dev/dotfiles/.ideavimrc`), apart from the keys listed first. "Both" means the key behaves the same in Neovim and IntelliJ.

## Your setup: what differs from stock Vim

These keys behave differently from a stock Vim tutorial. Check this table before trusting muscle memory from elsewhere.

| Key | Stock Vim | In your setup | Where |
| --- | --- | --- | --- |
| `Space` | move right | leader key | Both |
| `s` | substitute character | flash jump: type characters, press the label | Both (use `cl` to substitute) |
| `S` | substitute line | flash select: grows the selection outward through enclosing code blocks, labelled | Both (use `cc`) |
| `f` `F` `t` `T` | find character on the line | crosses lines, highlights matches, press again to repeat | Both (normal/visual only in IntelliJ) |
| `d` `D` `x` `X` `c` `C` | delete into the register | delete without yanking; name a register to keep it (`"ad`) | Both |
| `p` in visual | paste, then yank the replaced text | paste and keep the clipboard | Both |
| `y` / `p` | unnamed register | system clipboard | Both |
| `H` / `L` | top / bottom of screen | previous / next buffer | Both |
| `W` `B` `E` | WORD motions | camelCase hump motions | IntelliJ only |
| `n` / `N` | direction of last `/` or `?` | `n` always forward, `N` always back | Both |
| `Y` | yank line | yank to end of line | Both |
| `Esc` (normal) | nothing | clear search highlight | Both |
| `<C-h/j/k/l>` | various | move between windows | Both |
| `<C-s>` | nothing | save (save all in IntelliJ) | Both |
| `<C-/>` | nothing | toggle terminal | Both |
| `K` | look up keyword | hover documentation | Both |

In IntelliJ the Ctrl keys belong to Vim, so IDE Ctrl shortcuts moved to the leader (Ctrl+E is `<leader>fr`, Ctrl+K is `<leader>gs`). Autoformat is off in Neovim, so format on demand with `<leader>cf`.

## Motions

Any motion takes a count (`3w`, `5j`) and works after an operator (`d3w`, `y}`).

| Keys | Moves to |
| --- | --- |
| `w` / `b` / `e` / `ge` | next word start / previous word start / word end / previous word end |
| `0` / `^` / `$` / `g_` | line start / first non-blank / line end / last non-blank |
| `gg` / `G` / `42G` | first line / last line / line 42 |
| `M` | middle of the screen (`H`/`L` are buffers here) |
| `{` / `}` | previous / next blank line (paragraph) |
| `%` | matching bracket, or `if`/`else`, XML tag pair |
| `*` / `#` | next / previous occurrence of the word under the cursor |
| `[m` / `]m` | previous / next method start |
| `]f` `[f` / `]c` `[c` / `]a` `[a` | next/previous function / class / argument |
| `]]` / `[[` | next / previous reference to the symbol under the cursor |
| `]d` `[d` / `]e` `[e` | next / previous diagnostic / error |
| `]h` / `[h` | next / previous git hunk |
| `]q` / `[q` | next / previous quickfix item (IntelliJ: find/usage result) |
| `]t` / `[t` | next / previous TODO/FIXME comment |
| `<C-d>` / `<C-u>` | half page down / up |
| `<C-f>` / `<C-b>` | page down / up |
| `zz` / `zt` / `zb` | scroll so the cursor line is centred / at top / at bottom |
| `<C-e>` / `<C-y>` | scroll one line, cursor stays |

## Operators and editing

An operator followed by a motion or text object is the core grammar: `d2w`, `ci(`, `yap`, `gUiw`. Doubling the operator acts on the line (`dd`, `cc`, `>>`).

| Keys | Does |
| --- | --- |
| `d` / `c` / `y` + motion | delete / change / yank (`d` and `c` don't yank here) |
| `dd` / `cc` / `yy` | delete / change / yank the line |
| `D` / `C` | delete / change to end of line |
| `>` `<` + motion, `>>` `<<` | indent / dedent |
| `=` + motion, `==` | reindent |
| `gu` / `gU` / `g~` + motion, `~` | lowercase / uppercase / toggle case, toggle one character |
| `gc` + motion, `gcc` | toggle comment |
| `gco` / `gcO` | add a comment below / above |
| `.` | repeat the last change (takes a count: `3.`) |
| `u` / `<C-r>` | undo / redo |
| `J` / `gJ` | join lines (IntelliJ joins smartly, e.g. merges strings) / join without a space |
| `r{char}` / `R` | replace one character / replace mode |
| `<C-a>` / `<C-x>` | increment / decrement the number at or after the cursor |
| `<A-j>` / `<A-k>` | move line or selection down / up |
| `ys{motion}{char}` / `cs{old}{new}` / `ds{char}` | add / change / delete surrounding quotes or brackets (IntelliJ only) |

Two classic tricks break because `x` and `d` don't yank here. Swap characters with `"zx"zp` instead of `xp`, and move lines with `<A-j>` / `<A-k>` instead of `ddp`.

## Text objects

Use `i` (inside) or `a` (around) plus a letter after an operator or in visual mode: `daf` deletes a function, `cia` changes an argument, `yag` yanks the whole file, `ciq` changes inside the nearest quotes.

Built in everywhere: `iw` word, `iW` WORD, `ip` paragraph, `i"` `i'` `` i` `` quotes, `i(` `i{` `i[` `i<` brackets, `it` tag.

| Letter | Neovim (mini.ai) | IntelliJ (AnyObject, mini-ai) |
| --- | --- | --- |
| `q` | any quote | any quote |
| `b` | any bracket | any bracket |
| `f` | function | function |
| `c` | class | class |
| `a` | argument | argument |
| `o` | block, conditional or loop | any bracket (a `{}` block in Java) |
| `g` | whole buffer | whole buffer |
| `t` | tag | tag |
| `u` | function call | camelCase subword |
| `e` | camelCase word segment | none |
| `d` | number | none |
| `k` | none | comment |
| `s` | sentence | string |
| `v` | none | variable |
| `i` | none | list item |
| `m` | none | lambda or function |
| `l` / `y` | none | loop / conditional |
| `n` | none | indent block |

In Neovim, `n` and `l` after `i`/`a` mean next and last: `cinq` changes inside the next quotes, `dal)` deletes the previous parentheses. In IntelliJ `in` and `il` are the indent-block and loop objects instead.

## Search and replace

Search ignores case unless the pattern has a capital letter. Search first with `/` or `*`, then `:%s//new/g` reuses that pattern.

| Keys / command | Does |
| --- | --- |
| `/text` / `?text` | search forward / backward |
| `*` / `#` | search the word under the cursor, whole word |
| `g*` / `g#` | same, partial matches too |
| `*` in visual | search the selected text |
| `:s/old/new/` | replace the first match on the line |
| `:%s/old/new/g` | replace every match in the file |
| `:%s/old/new/gc` | same, confirm each one |
| `:'<,'>s/old/new/g` | replace inside the selection (press `:` in visual) |
| `:%s//new/g` | reuse the last search pattern |
| `:%s/\v(\w+)Id/\1Key/g` | capture group; `\v` saves the backslashes |
| `\<word\>` | match whole word only |
| `&` / `g&` | repeat the last `:s` on this line / on every line |
| `:g/pattern/d` / `:v/pattern/d` | delete lines that match / don't match |
| `:g/pattern/normal @q` | run macro `q` on every matching line |
| `<leader>/` / `<leader>sw` | grep the project / grep the word or selection |
| `<leader>sr` | search and replace across the project |

Substitute flags: `g` every match on the line, `c` confirm, `i` ignore case, `I` match case, `n` count only.

## Registers, yank and paste

Yank goes to the system clipboard. Delete and change throw the text away. To cut and paste, name a register: `"add` cuts the line into `a`, then `"ap` pastes it.

| Register | Holds |
| --- | --- |
| `"+` | system clipboard, the default for `y` and `p` here |
| `"0` | last yank |
| `"a` to `"z` | named registers: `"ayiw` yanks a word into `a`, `"ap` pastes it |
| `"A` to `"Z` | append to the named register |
| `"_` | black hole, used by `d` `c` `x` here |
| `"1` to `"9` | older deletes (mostly empty here) |
| `".` | last inserted text |
| `":` | last command line (`@:` runs it again) |
| `"/` | last search pattern |
| `"%` | current file name |

| Keys | Does |
| --- | --- |
| `p` / `P` | paste after / before the cursor |
| `gp` / `gP` | paste and leave the cursor after the pasted text |
| `p` in visual | replace the selection, clipboard unchanged |
| `"` (wait) | popup listing every register's contents |
| `:reg` / `<leader>s"` | list registers |
| `<C-r>{reg}` | in insert mode or the command line, paste that register |
| `<C-r><C-w>` | on the command line, insert the word under the cursor (Neovim) |

## Marks and jumps

``` `` ``` and `<C-o>` are the two to learn first: they undo any jump, including `gd`, `G`, `%` and searches.

| Keys | Does |
| --- | --- |
| `ma` | set mark `a` in this file |
| `` `a `` / `'a` | jump to mark `a` exactly / to its line |
| `mA` | file mark, works across files (a bookmark in IntelliJ) |
| ``` `` ``` / `''` | back to the position / line before the last jump |
| `<C-o>` / `<C-i>` | back / forward through the jump list |
| `` `. `` | where the last change happened |
| `` `[ `` / `` `] `` | start / end of the last yank or change |
| `g;` / `g,` | older / newer change position |
| `gi` | go back into insert mode where you last left it |
| `gv` | reselect the last visual selection |
| `:marks` / `<leader>sm` | list marks |
| `<leader>sj` | jump list (Recent Locations in IntelliJ) |

## Macros and repetition

Start a macro with `0` or `^` and end it with `j`. Then `100@a` walks down the file and stops at the first line where it fails.

| Keys / command | Does |
| --- | --- |
| `qa` ... `q` | record keystrokes into register `a` |
| `qA` ... `q` | append to macro `a` |
| `@a` / `10@a` | play macro `a` once / 10 times |
| `@@` | replay the last macro |
| `.` | repeat the last change |
| `@:` | repeat the last `:` command |
| `:'<,'>normal @a` | play `a` on every selected line |
| `:%normal A;` | run normal-mode keys on every line (here: append `;`) |
| `:g/pattern/normal @a` | play `a` on every matching line |
| `"ap`, edit, `0"ay$` | fix a macro: paste it as text, edit, yank it back |

In IntelliJ, macros that open IDE popups (rename, refactor menus) don't replay reliably. Keep them to plain Vim editing.

## Visual and block mode

Block mode (`<C-v>`) edits a column on many lines at once: select it, press `I` or `A`, type, then `Esc` copies the edit to every line.

| Keys | Does |
| --- | --- |
| `v` / `V` / `<C-v>` | select characters / lines / a block |
| `o` | jump to the other end of the selection |
| `gv` | reselect the last selection |
| `<C-v>`, `I` text `Esc` | insert the text before the block on every line |
| `<C-v>`, `A` text `Esc` | append the text after the block on every line |
| `<C-v>`, `$`, `A` text `Esc` | append at the end of every line, ragged lengths too |
| `c` / `r{char}` | change the selection / replace every character in it |
| `>` / `<` | indent / dedent; the selection stays, so press again |
| `J` | join the selected lines |
| `u` / `U` / `~` | lowercase / uppercase / toggle case |
| `<C-a>` / `g<C-a>` | increment every number / turn them into 1, 2, 3 ... |
| `:` | command over the selected lines (`:'<,'>`) |
| `*` | search for the selected text |
| `S` | flash select: pick an enclosing code block by label |
| `gsa{char}` | surround the selection with quotes or brackets (IntelliJ only; `S` is flash) |
| `<C-Space>` / `<BS>` | grow / shrink the selection by code structure (IntelliJ) |

## Insert mode

Enter insert with `i` `a` (before/after cursor), `I` `A` (line start/end), `o` `O` (new line below/above), `C` (change to end of line) or `gi` (where you left off). Because `s` and `S` are flash here, substitute with `cl` and `cc`.

| Keys | Does |
| --- | --- |
| `Esc` / `<C-c>` / `<C-[>` | back to normal mode |
| `<C-w>` / `<C-u>` | delete the word before the cursor / back to line start |
| `<C-h>` | backspace |
| `<C-o>` | run one normal-mode command, then keep typing (`<C-o>zz`) |
| `<C-r>{reg}` | paste a register: `<C-r>+` clipboard, `<C-r>0` last yank |
| `<C-Space>` | open completion |
| `<C-n>` / `<C-p>` | next / previous completion item |
| `<C-y>` or `Enter` | accept completion |
| `<C-e>` | close completion |
| `<C-k>` | signature help (parameter info) |
| `<C-s>` | save and return to normal mode |
| `<A-j>` / `<A-k>` | move the line down / up |
| `<C-v>` | paste in IntelliJ; insert a literal character in Neovim |

## Windows, buffers and folds

A buffer is an open file (an editor tab in IntelliJ); a window is a split showing one. Split with `<leader>-` (below) or leader then `|` (right), and move between splits with `<C-h/j/k/l>`.

| Keys / command | Does |
| --- | --- |
| `<C-w>s` / `<C-w>v` | split below / right |
| `<C-w>w` | cycle to the next window |
| `<C-w>c` / `<leader>wd` | close this window |
| `<C-w>o` | close every other window |
| `<C-w>=` | equalise window sizes (Neovim) |
| `<C-Up>` `<C-Down>` `<C-Left>` `<C-Right>` | resize the window |
| `<leader>wm` | zoom: maximise the editor (IntelliJ hides tool windows) |
| `H` / `L` or `[b` / `]b` | previous / next buffer |
| `<leader>bb` | switch to the alternate buffer |
| `<leader>bd` / `<leader>bo` | close this buffer / close all others |
| `<leader>,` | buffer picker |
| `:e path` / `:e #` | open a file / reopen the alternate file |
| `:w` / `:wa` / `:q` / `:qa` | save / save all / close / quit all |
| `ZZ` / `ZQ` | save and close / close without saving |
| `za` / `zc` / `zo` | toggle / close / open the fold under the cursor |
| `zR` / `zM` | open / close every fold |

## Leader keys

Press `Space` and wait: which-key lists everything under it. Groups: `b` buffer, `c` code, `f` find, `g` git, `s` search, `u` ui, `w` windows, `x` diagnostics, `a` ai, plus IntelliJ-only `d` debug, `t` test, `o` run/Spring, `r` refactor, `R` HTTP requests.

### Everyday

| Keys | Does |
| --- | --- |
| `<leader><Space>` | find files |
| `<leader>,` / `<leader>fr` | open buffers / recent files |
| `<leader>/` | grep the project |
| `<leader>e` | file explorer |
| `<leader>fn` | new file |
| `<leader>ft` or `<C-/>` | terminal |
| `<leader>ss` / `<leader>sS` | symbols in this file / in the workspace |
| `<leader>sk` | list keymaps |
| `<leader>n` | notification history |
| `<leader>qq` | quit |

### Code

| Keys | Does |
| --- | --- |
| `gd` / `gr` / `gI` / `gy` | definition / references / implementation / type definition |
| `K` / `gK` | documentation / signature |
| `<leader>ca` | code action (IntelliJ's Alt+Enter menu) |
| `<leader>cA` | source action (IntelliJ: Generate getters, constructors, overrides) |
| `<leader>cr` / `<leader>cR` | rename symbol / rename file |
| `<leader>cf` / `<leader>co` | format / organize imports |
| `<leader>cd` / `<leader>xx` | diagnostics for this line / diagnostics list |
| `<leader>cxv` / `<leader>cxc` / `<leader>cxm` | extract variable / constant / method |
| `<leader>cgs` / `<leader>cgS` | go to super method / jump between test and class |
| `<leader>rs` | refactor menu (IntelliJ) |

### Git

| Keys | Does |
| --- | --- |
| `<leader>gg` / `<leader>gG` | lazygit at the project root / the file's directory |
| `<leader>gs` | git status (IntelliJ: Commit window) |
| `<leader>gb` | blame |
| `<leader>gf` / `<leader>gl` | file history / log |
| `<leader>gB` | open on GitHub |
| `<leader>ghp` / `<leader>ghr` | preview / reset the hunk |
| `<leader>gc` | branches (IntelliJ) |

### Debug, test and run (IntelliJ)

| Keys | Does |
| --- | --- |
| `<leader>db` / `<leader>dB` | toggle breakpoint / conditional breakpoint |
| `<leader>dc` | continue (F9 is taken by dictation) |
| `<leader>di` / `<leader>dO` / `<leader>do` | step into / over / out |
| `<leader>de` | evaluate the expression under the cursor |
| `<leader>dh` | hot-swap changed classes |
| `<leader>tr` / `<leader>tt` / `<leader>td` | run nearest test / tests in file / debug nearest |
| `<leader>tl` / `<leader>tf` | rerun last / rerun failed |
| `<leader>oo` / `<leader>oO` / `<leader>os` | run / pick a run configuration / stop |
| `<leader>ou` | update the running app, as its run configuration's "On Update" setting says |
| `<leader>ow` | Services window (running Spring Boot apps) |
| `<leader>oe` / `<leader>oB` / `<leader>op` | Endpoints / Spring Beans / active profiles |
| `<leader>oR` | reload Maven/Gradle projects |
| `<leader>Rs` / `<leader>RS` | send the HTTP request under the cursor / all requests |

### AI and tools

| Keys | Does |
| --- | --- |
| `<leader>ac` | toggle Claude Code |
| `<leader>ab` / `<leader>as` | add this file / send the selection to Claude |
| `<leader>aa` / `<leader>ad` | accept / deny Claude's proposed diff (Neovim) |
| `<leader>D` | database UI |
| `<leader>l` | plugin manager |
| `<leader>uw` / `<leader>uL` / `<leader>uz` | toggle wrap / relative numbers / zen mode |
