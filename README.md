# Tmux Configuration — Complete Usage Guide

This README documents your tmux setup as configured in `~/.config/tmux/tmux.conf`. It includes key bindings, plugin documentation, session management, and installation---

## Session Persistence & Recovery

Your tmux setup includes powerful session persistence through two complementary plugins:

### Automatic Session Management
* **tmux-continuum** automatically saves your session every 15 minutes
* Sessions are **automatically restored** when you start tmux
* Works silently in the background - no action needed from you
* Your work environment persists across reboots and tmux restarts

### Manual Session Control
* **Quick save**: `Prefix` + `Ctrl-s` (saves immediately)
* **Quick restore**: `Prefix` + `Ctrl-r` (restores last saved state)
* Useful for creating manual checkpoints before major changes

### What Gets Preserved
* **Layout**: All windows, panes, and their exact arrangement
* **Directories**: Each pane's current working directory
* **Content**: Visible text in each pane (scrollback)
* **Programs**: Running processes in each pane
* **Editor sessions**: Vim/Neovim sessions (when using session files)

### File Locations
* Session data stored in: `~/.tmux/resurrect/`
* Last saved session: `~/.tmux/resurrect/last`
* Automatic saves: `~/.tmux/resurrect/tmux_resurrect_*.txt`

---

## Tips & Troubleshooting

### General Issues
* **Alt‑arrow keys don't work?** Your terminal may capture them. Disable "Alt sends escape" or similar in terminal settings, or use Vim‑style `Prefix` + `h/j/k/l` instead.
* **Colors look wrong?** If `tmux-256color` is unknown on remote hosts, temporarily set `set -g default-terminal "xterm-256color"` or install ncurses-term package.

### Clipboard Issues  
* **tmux-yank not working?** Ensure you have the right clipboard tool:
  - **X11**: Install `xclip` or `xsel`
  - **Wayland**: Install `wl-clipboard` (`wl-copy`, `wl-paste`)
  - **SSH**: May need X11 forwarding (`ssh -X`) or remote clipboard tools

### Session Persistence Issues
* **Sessions not auto-restoring?** Check that tmux-continuum is installed: `ls ~/.tmux/plugins/tmux-continuum`
* **Manual restore not working?** Verify session files exist: `ls ~/.tmux/resurrect/`
* **Programs not restored correctly?** Some programs may need manual restart; resurrect focuses on layout and basic processes
* **Vim sessions not working?** Ensure you're using `:mksession` in Vim/Neovim to create session files

### Nested tmux Sessions
* **Send prefix to inner tmux**: Press `Prefix` twice (`Ctrl-b` then `Ctrl-b` again)
* Useful when SSH'd into another machine running tmuxuctions.

> **Prefix**: `Ctrl-b` (default tmux prefix)

---

## Quick Reference Cheat Sheet

### Pane Navigation
* **No prefix**: `Alt-←/→/↑/↓` → move focus between panes
* **Vim-style** (with prefix): `Prefix` + `h/j/k/l` → move left/down/up/right

### Window Management
* **Switch windows**: `Shift-←/→` or `Alt-H/L` *(no prefix needed)*
* Windows and panes start at **1**; auto-renumber when closed

### Splitting Panes (preserves current directory)
* **Vertical split**: `Prefix` + `"`
* **Horizontal split**: `Prefix` + `%`

### Copy Mode (vi-style)
* **Enter copy-mode**: `Prefix` + `[`
* **Start selection**: `v` · **Rectangle**: `Ctrl-v` · **Copy & exit**: `y`
* **Paste**: `Prefix` + `]`

### Session Management & Persistence
* **Save session**: `Prefix` + `Ctrl-s` *(manual save)*
* **Restore session**: `Prefix` + `Ctrl-r` *(manual restore)*
* **Auto-save**: Every 15 minutes *(automatic with continuum)*
* **Auto-restore**: On tmux startup *(automatic with continuum)*

### Basic Session Commands
* **Detach**: `Prefix` + `d`
* **List sessions**: `tmux ls`
* **Attach to session**: `tmux attach -t <name>`
* **Create new session**: `tmux new -s <name>`

---

## Core Behavior

### Terminal & Colors

* `default-terminal` is set to `tmux-256color` (truecolor).

  * If your terminal/remote host complains about unknown terminfo, switch to `xterm-256color` or install the `tmux-256color` terminfo.
* `terminal-overrides` enables 24‑bit color (`Tc`).
* `escape-time` is `0` for snappier key handling (helps in Vim/Neovim).
* Mouse support is **on** (resize/select panes, click to focus, scroll in copy-mode, etc.).

### Prefix & Navigation

* Prefix is **`Ctrl-b`** (default tmux prefix).
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

### tmux-resurrect

Saves and restores tmux sessions, including:
- All sessions, windows, and pane layouts
- Each pane's current working directory  
- Pane contents (text displayed in each pane)
- Running programs in each pane
- Vim/Neovim sessions (when properly configured)

**Manual Usage:**
* **Save session**: `Prefix` + `Ctrl-s`
* **Restore session**: `Prefix` + `Ctrl-r`

**What gets saved:**
- Window and pane structure
- Current working directories
- Pane contents (terminal output)
- Running processes
- Vim/Neovim sessions (with session files)

---

### tmux-continuum

Automatic session management that works with tmux-resurrect:
- **Auto-saves** sessions every 15 minutes
- **Auto-restores** sessions when tmux starts
- Works seamlessly in the background

**Features:**
- No manual intervention needed
- Sessions persist across system reboots
- Configurable save intervals
- Status line integration available

**Configuration:**
- Save interval: 15 minutes
- Auto-restore: Enabled on tmux startup
- Works with tmux-resurrect settings

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

## Command Reference (Complete List)

**Prefix**: `Ctrl-b`

### Pane Navigation
**No prefix required:**
* `Alt-←/→/↑/↓` → select pane

**With prefix:**
* `h` / `j` / `k` / `l` → select pane Left/Down/Up/Right

### Window Management
**No prefix required:**
* `Shift-←` / `Shift-→` → previous / next window
* `Alt-H` / `Alt-L` → previous / next window

### Pane Splitting (preserves current directory)
* `Prefix` + `"` → vertical split
* `Prefix` + `%` → horizontal split

### Copy Mode (vi-style)
* **Enter**: `Prefix` + `[` · **Paste**: `Prefix` + `]`
* **In copy-mode**: `v` select · `Ctrl-v` rectangle · `y` copy & exit

### Session Management
**Basic sessions:**
* **Detach**: `Prefix` + `d`
* **List**: `tmux ls`
* **Attach**: `tmux attach -t <name>`
* **New**: `tmux new -s <name>`

**Session persistence (tmux-resurrect):**
* **Save session**: `Prefix` + `Ctrl-s`
* **Restore session**: `Prefix` + `Ctrl-r`
* **Auto-save**: Every 15 minutes (tmux-continuum)
* **Auto-restore**: On tmux startup (tmux-continuum)

### Plugin Management (TPM)
* **Install plugins**: `Prefix` + `I`
* **Update plugins**: `Prefix` + `U` 
* **Clean unused**: `Prefix` + `Alt-u`

---


