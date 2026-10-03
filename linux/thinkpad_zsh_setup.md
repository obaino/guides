# ThinkPad T14 — Zsh Setup

## 1. Purpose

This document describes the current Zsh setup on the ThinkPad T14 and provides a reproducible reference for rebuilding it after a Linux Mint installation.

The configuration is split between local Zsh startup files, configuration files stored in Dropbox, and plugin repositories stored under `~/.local/src`.

## 2. System / Zsh

- Zsh 5.9
- Default shell: `/usr/bin/zsh`
- `ZDOTDIR`: `~/.config/zsh`

## 3. Zsh startup architecture

```text
~/.zshenv
    |
    +-- sets ZDOTDIR
    +-- sets user PATH
    |
    v
~/.config/zsh/
    |
    +-- .zshrc
    +-- .zprofile
    +-- .p10k.zsh
    +-- .aliasrc
    +-- .optionrc
    |
    +-- plugins/
```

`~/.zshenv` is outside `ZDOTDIR` because it must be found before `ZDOTDIR` can redirect the remaining Zsh configuration files.

### `~/.zshenv`

The intended current contents are:

```zsh
export ZDOTDIR="$HOME/.config/zsh"
export PATH="$HOME/bin:$HOME/.local/bin:$PATH"
```

The PATH entries are placed here so that `~/bin` and `~/.local/bin` are available in ordinary interactive Zsh terminals, not only login shells.

## 4. Configuration directory

The main Zsh configuration directory is:

```text
~/.config/zsh/
```

Current structure:

```text
~/.config/zsh/
├── .aliasrc
├── .optionrc
├── .p10k.zsh
├── .zprofile
├── .zshrc
└── plugins/
```

The five configuration files are symbolic links to copies stored in Dropbox.

### Dropbox location

```text
~/DropboxMain/settings/LinuxMInt/zsh/
```

Current links:

```text
~/.config/zsh/.aliasrc
    -> ~/DropboxMain/settings/LinuxMInt/zsh/thinkpad_aliasrc

~/.config/zsh/.optionrc
    -> ~/DropboxMain/settings/LinuxMInt/zsh/thinkpad_optionrc

~/.config/zsh/.p10k.zsh
    -> ~/DropboxMain/settings/LinuxMInt/zsh/thinkpad_p10k.zsh

~/.config/zsh/.zprofile
    -> ~/DropboxMain/settings/LinuxMInt/zsh/thinkpad_zprofile

~/.config/zsh/.zshrc
    -> ~/DropboxMain/settings/LinuxMInt/zsh/thinkpad_zshrc
```

## 5. `.zshrc`

The main interactive-shell configuration is `~/.config/zsh/.zshrc`.

It:

1. Loads Powerlevel10k configuration.
2. Loads aliases and shell options.
3. Configures history.
4. Configures completion.
5. Loads zsh-autosuggestions.
6. Loads zsh-history-substring-search.
7. Loads fast-syntax-highlighting.
8. Loads the Powerlevel10k theme.
9. Sets the terminal title.

The plugin loading order is deliberate:

```text
zsh-autosuggestions
        |
zsh-history-substring-search
        |
fast-syntax-highlighting
        |
Powerlevel10k theme
```

Fast syntax highlighting is loaded after the other widgets/plugins.

The terminal title is updated with `precmd` and `preexec`: the current directory is shown at the prompt and the running command is shown while a command executes.

## 6. `.zprofile`

Current file:

```text
~/.config/zsh/.zprofile
```

Contents:

```zsh
# User-local executables
export PATH="$HOME/bin:$HOME/.local/bin:$PATH"
```

The same PATH is also exported by `.zshenv`.

This preserves the existing login-shell configuration while `.zshenv` guarantees the PATH for ordinary Zsh invocations.

## 7. History

History file:

```text
~/.local/state/zsh/history
```

Configuration:

```zsh
HISTFILE="${XDG_STATE_HOME:-$HOME/.local/state}/zsh/history"
HISTSIZE=10000
SAVEHIST=10000
```

History options include:

```zsh
APPEND_HISTORY
EXTENDED_HISTORY
HIST_EXPIRE_DUPS_FIRST
HIST_FIND_NO_DUPS
HIST_IGNORE_DUPS
HIST_IGNORE_SPACE
HIST_REDUCE_BLANKS
HIST_SAVE_NO_DUPS
SHARE_HISTORY
```

Additional history options are in `.optionrc`:

```zsh
HIST_IGNORE_ALL_DUPS
HIST_SAVE_NO_DUPS
HIST_REDUCE_BLANKS
HIST_VERIFY
```

## 8. Completion

Completion is enabled with:

```zsh
autoload -Uz compinit
```

The configuration provides case-insensitive matching and menu selection.

The completion dump is stored at:

```text
~/.cache/zsh/.zcompdump
```

## 9. Plugins

Plugin links under:

```text
~/.config/zsh/plugins/
```

are:

```text
fast-syntax-highlighting -> ~/.local/src/fast-syntax-highlighting
powerlevel10k            -> ~/.local/src/powerlevel10k
zsh-autosuggestions      -> ~/.local/src/zsh-autosuggestions
zsh-history-substring-search
                         -> ~/.local/src/zsh-history-substring-search
```

## 10. Local source repositories

Current `~/.local/src/` contains:

```text
catppuccin_gnome-terminal
catppuccin_xed
fast-syntax-highlighting
Nordic-Polar
powerlevel10k
zsh-autosuggestions
zsh-history-substring-search
```

The four repositories used directly by Zsh are:

```text
fast-syntax-highlighting
powerlevel10k
zsh-autosuggestions
zsh-history-substring-search
```

`Nordic-Polar` is a separate theme repository, not a Zsh plugin.

## 11. Powerlevel10k

Repository:

```text
~/.local/src/powerlevel10k
```

Plugin link:

```text
~/.config/zsh/plugins/powerlevel10k
```

Configuration:

```text
~/.config/zsh/.p10k.zsh
```

Dropbox copy:

```text
~/DropboxMain/settings/LinuxMInt/zsh/thinkpad_p10k.zsh
```

The generated P10k configuration should be treated as a separate configuration file rather than manually rebuilt line-by-line.

## 12. Aliases

Aliases are stored in:

```text
~/.config/zsh/.aliasrc
```

with the Dropbox copy at:

```text
~/DropboxMain/settings/LinuxMInt/zsh/thinkpad_aliasrc
```

Main groups:

- Navigation: `..`, `...`, `....`
- Safer file operations: `cp`, `mv`, `rm`
- System updates: `update`, `upgrade`, `autoclean`, `autoremove`
- Listings: `ls`, `ll`, `la`, `l`, `l.`
- Zsh editing: `edit-aliases`, `edit-zshrc`, `edit-zprofile`, `edit-options`
- Monitoring: `vtemp`, `verrors`, `vkerrors`
- Maestral: `dropbox-status`, `dropbox-start`, `dropbox-stop`
- Other commands: `grep`, `history`, `hsi`, `lsblkf`, `clip`, `sdata`, `usbstatus`, `market`

The configuration-editing aliases deliberately use Xed.

## 13. Shell options

`.optionrc` contains:

### History

```zsh
HIST_IGNORE_ALL_DUPS
HIST_SAVE_NO_DUPS
HIST_REDUCE_BLANKS
HIST_VERIFY
```

### Directory navigation

```zsh
AUTO_CD
PUSHD_IGNORE_DUPS
PUSHD_SILENT
```

### Completion

```zsh
AUTO_MENU
COMPLETE_IN_WORD
```

### Globbing

```zsh
EXTENDED_GLOB
```

## 14. PATH

Personal executable directories:

```text
~/bin
~/.local/bin
```

They are at the beginning of PATH:

```text
/home/nikolask/bin
/home/nikolask/.local/bin
```

This allows local scripts and commands to be run without their full path.

## 15. Reproduction procedure

For a fresh Linux Mint installation:

### Step 1 — install Zsh

Install the distribution Zsh package and verify:

```bash
zsh --version
```

### Step 2 — make Zsh the default shell

Verify:

```bash
getent passwd "$USER" | cut -d: -f7
```

Expected:

```text
/usr/bin/zsh
```

### Step 3 — create the configuration directories

```bash
mkdir -p ~/.config/zsh/plugins
mkdir -p ~/.local/src
```

### Step 4 — create `.zshenv`

Create `~/.zshenv` containing:

```zsh
export ZDOTDIR="$HOME/.config/zsh"
export PATH="$HOME/bin:$HOME/.local/bin:$PATH"
```

### Step 5 — restore the Dropbox configuration

The authoritative copies are stored in:

```text
~/DropboxMain/settings/LinuxMInt/zsh/
```

Link these files into `~/.config/zsh/`:

```text
.aliasrc
.optionrc
.p10k.zsh
.zprofile
.zshrc
```

### Step 6 — restore the plugin repositories

The four required repositories must exist under:

```text
~/.local/src/
```

Then recreate the four plugin links under:

```text
~/.config/zsh/plugins/
```

### Step 7 — verify

Open a new terminal:

```bash
echo "$ZDOTDIR"
echo "$PATH"
```

Expected:

```text
/home/nikolask/.config/zsh
```

and PATH beginning with:

```text
/home/nikolask/bin:/home/nikolask/.local/bin:
```

Verify:

- Powerlevel10k prompt appears.
- Tab completion works.
- Autosuggestions appear.
- Up/down arrows perform history substring search.
- Syntax highlighting works.
- `ll` works.
- `edit-zshrc` opens the configuration with Xed.

## 16. Maintenance rules

### Do not edit `/etc/zsh/zshenv`

User-specific environment configuration belongs in:

```text
~/.zshenv
```

### Do not replace the plugin links with copied plugin files

The links connect the Zsh configuration to the repositories under:

```text
~/.local/src/
```

### Do not manually rebuild the generated P10k configuration

The P10k configuration is stored separately and synchronized through Dropbox.

### Use Xed for configuration editing

Current aliases include:

```text
edit-aliases
edit-zshrc
edit-zprofile
edit-options
```

## 17. Quick diagnostic checklist

If Zsh stops behaving as expected:

```bash
zsh --version
getent passwd "$USER" | cut -d: -f7
echo "$ZDOTDIR"
echo "$PATH"
```

Then:

```bash
ls -la ~/.zshenv ~/.config/zsh/
ls -la ~/.config/zsh/plugins/
```

Check configuration links:

```bash
readlink -f ~/.config/zsh/.zshrc
readlink -f ~/.config/zsh/.zprofile
readlink -f ~/.config/zsh/.aliasrc
readlink -f ~/.config/zsh/.optionrc
readlink -f ~/.config/zsh/.p10k.zsh
```

Check plugin links:

```bash
readlink -f ~/.config/zsh/plugins/fast-syntax-highlighting
readlink -f ~/.config/zsh/plugins/powerlevel10k
readlink -f ~/.config/zsh/plugins/zsh-autosuggestions
readlink -f ~/.config/zsh/plugins/zsh-history-substring-search
```

## 18. Current architecture

```text
                         Dropbox
                           |
                           v
~/DropboxMain/settings/LinuxMInt/zsh/
    |
    +-- thinkpad_zshrc
    +-- thinkpad_zprofile
    +-- thinkpad_aliasrc
    +-- thinkpad_optionrc
    +-- thinkpad_p10k.zsh
                           |
                           | symbolic links
                           v
                    ~/.config/zsh/
                    |
                    +-- .zshrc
                    +-- .zprofile
                    +-- .aliasrc
                    +-- .optionrc
                    +-- .p10k.zsh
                    |
                    +-- plugins/
                           |
                           | symbolic links
                           v
                       ~/.local/src/
                           |
                           +-- fast-syntax-highlighting
                           +-- powerlevel10k
                           +-- zsh-autosuggestions
                           +-- zsh-history-substring-search
```

The one Zsh configuration file outside this tree is:

```text
~/.zshenv
```

because it establishes `ZDOTDIR` before the rest of the Zsh configuration is read.

## 19. Verification date

This documentation reflects the ThinkPad configuration inspected on:

```text
2026-10-03
```

Re-check the configuration after significant Zsh, Powerlevel10k, plugin, shell, or Linux Mint changes.
