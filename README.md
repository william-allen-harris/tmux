# Tmux Setup — Commands & Usage Guide

This README documents how to use your tmux setup as configured in `~/.config/tmux/tmux.conf`. It includes a quick-start cheat sheet, plugin sections, and installation instructions. **All key bindings below reflect your exact config.**

> **Prefix**: `Ctrl-Space` (you unbound the default `Ctrl-b`).

---

## TL;DR Cheat Sheet (Most Used)

**Pane navigation**

* With *no* prefix: `Alt-←/→/↑/↓` → move focus left/right/up/down
* With prefix (Vim-style): `h/j/k/l` → move left/down/up/right

**Windows**

* Previous / Next window: `Shift-←` / `Shift-→` *(no prefix)*
* Also prev/next: `Alt-H` / `Alt-L` *(no prefix)*
* Windows and panes start at **1**; numbers renumber automatically on close

**Splits (keep current directory)**

* Vertical split: `Prefix` + `"`
* Horizontal split: `Prefix` + `%`

**Copy mode (vi)**

* Enter copy-mode: `Prefix` + `[`
* Start selection: `v`  ·  Rectangle toggle: `Ctrl-v`  ·  Copy + exit: `y`
* Paste last buffer: `Prefix` + `]`

**Detach / Sessions**

* Detach: `Prefix` + `d`
* List sessions: `tmux ls`
* Attach to session: `tmux attach -t <name>`
* Create new: `tmux new -s <name>`

**Nested tmux (send prefix to inner)**

* Send a literal prefix: `Prefix` then `Ctrl-Space` (i.e., **`Ctrl-Space` → `Ctrl-Space`**)

---

## Core Behavior

### Terminal & Colors

* `default-terminal` is set to `tmux-256color` (truecolor).

  * If your terminal/remote host complains about unknown terminfo, switch to `xterm-256color` or install the `tmux-256color` terminfo.
* `terminal-overrides` enables 24‑bit color (`Tc`).
* `escape-time` is `0` for snappier key handling (helps in Vim/Neovim).
* Mouse support is **on** (resize/select panes, click to focus, scroll in copy-mode, etc.).

### Prefix & Navigation

* Prefix is **`Ctrl-Space`**.
* Vim-style pane moves **after prefix**: `h`, `j`, `k`, `l`.
* Global, no‑prefix pane moves via **Alt + arrows**.
* Window navigation shortcuts without prefix: `Shift-←/→` and `Alt-H/L`.

### Splits Keep CWD

Splits are re-bound to **preserve the current pane’s working directory**:

* `Prefix` + `"` → vertical split
* `Prefix` + `%`  → horizontal split

### Copy-mode (vi)

* Key table set to **vi**.
* In copy-mode: `v` to begin selection, `Ctrl-v` for rectangular selection, `y` to yank and exit.
* Paste with `Prefix` + `]`.

---

## Plugins

### TPM — Tmux Plugin Manager

Used to manage all plugins declared in the config.

**Key bindings (inside tmux):**

* Install plugins: `Prefix` + `I`
* Update plugins: `Prefix` + `U`
* Clean unused: `Prefix` + `Alt-u`

**Commands:**

* Reload config: `tmux source-file ~/.config/tmux/tmux.conf` (or see install notes to auto‑source)

---

### tmux-sensible

Applies a set of pragmatic defaults that complement your config. No keybinds to learn.

---

### vim-tmux-navigator

Unified navigation between **Neovim splits** and **tmux panes** using the same keys:

* `Ctrl-h`, `Ctrl-j`, `Ctrl-k`, `Ctrl-l`, and `Ctrl-\` (previous)

> You already mapped these in Neovim (`vim-tmux-navigator` on both sides). If movement doesn’t cross boundaries, ensure both plugins are installed and your terminal isn’t intercepting the keys.

---

### tmux-yank

Yanks from tmux copy-mode to the **system clipboard**.

**Workflow**

1. Enter copy-mode: `Prefix` + `[`.
2. Select with `v` (or `Ctrl-v` rectangle).
3. `y` to copy & exit.
4. Paste into other apps with your OS paste keys.

> On Linux/X11, install `xclip` or `xsel`. On Wayland, install `wl-clipboard` (`wl-copy`, `wl-paste`).

---

### monokai-pro.tmux

Applies a Monokai Pro theme to tmux. Loads automatically via TPM; no extra steps required.

---

## Installation

1. **Install tmux**

   ```bash
   sudo apt update && sudo apt install -y tmux
   ```

2. **Place the config**

   * Save your config at: `~/.config/tmux/tmux.conf`
   * Ensure tmux loads it automatically. If your tmux doesn’t read XDG paths by default, create a minimal `~/.tmux.conf` that sources it:

     ```tmux
     # ~/.tmux.conf
     source-file ~/.config/tmux/tmux.conf
     ```

3. **Install TPM (Plugin Manager)**

   ```bash
   git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
   ```

4. **Start tmux and install plugins**

   * Start tmux: `tmux`
   * Reload config (optional): `tmux source-file ~/.config/tmux/tmux.conf`
   * Press `Prefix` + `I` to fetch and install plugins.

5. **Verify truecolor** (optional)
   Inside tmux, run:

   ```bash
   echo $TERM           # should print tmux-256color
   ```

   If colors look wrong or `tmux-256color` is unknown on a remote host, use `xterm-256color` or install the terminfo there.

---

## Tips & Troubleshooting

* **Alt‑arrow keys don’t work?** Your terminal may capture them. Disable “Alt as meta for word navigation” or similar in your terminal settings, or use the Vim‑style `Prefix` + `h/j/k/l` instead.
* **Clipboard not working from tmux-yank?** Ensure you have `xclip`/`xsel` (X11) or `wl-clipboard` (Wayland) installed and available in `PATH`.
* **Nested tmux on SSH**: Use `Ctrl-Space`, then `Ctrl-Space` again to send the prefix to the inner session.
* **Unknown terminal type**: If `tmux-256color` is missing on a remote, either install the terminfo (`ncurses-term`) or temporarily set `set -g default-terminal "xterm-256color"`.

---

## Command Reference (Flat List)

**Prefix**: `Ctrl-Space`

**Pane focus (no prefix)**

* `Alt-←/→/↑/↓` → select pane

**Pane focus (with prefix)**

* `h` / `j` / `k` / `l` → select pane L/D/U/R

**Windows (no prefix)**

* `Shift-←` / `Shift-→` → previous / next window
* `Alt-H` / `Alt-L` → previous / next window

**Splits (keep CWD)**

* `Prefix` + `"` → vertical split
* `Prefix` + `%` → horizontal split

**Copy-mode (vi)**

* Enter: `Prefix` + `[`  ·  Paste: `Prefix` + `]`
* In copy-mode: `v` select · `Ctrl-v` rectangle · `y` copy & exit

**Sessions**

* Detach: `Prefix` + `d`
* List: `tmux ls`
* Attach: `tmux attach -t <name>`
* New: `tmux new -s <name>`

**TPM**

* Install: `Prefix` + `I` · Update: `Prefix` + `U` · Clean: `Prefix` + `Alt-u`

---


