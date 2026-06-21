# Linux post installation notes

## System Tweaks

### map mouse keys to switch desktop

```apt install xbindkeys```

edit ```~/.xbindkeysrc``` add the following:

```zsh
# Previous desktop
"dbus-send --session --type=method_call --dest=org.Cinnamon /org/Cinnamon org.Cinnamon.switchWorkspaceLeft"
  b:8

# Next desktop
"dbus-send --session --type=method_call --dest=org.Cinnamon /org/Cinnamon org.Cinnamon.switchWorkspaceRight"
  b:9
```

### keyboard repeat rate

Startup Applications add the following:

```zsh
xset r rate 300 50
```

### xed with privilege

```zsh
xed admin:///path/to/directory
```

### dropdown terminal

```zsh
gnome-terminal --geometry=192x24+200+10 --title='DropDown Terminal' --profile='IBM 3270 (High Contrast)'
```

```zsh
tilix -q -p gruvbox
```

## Applications

### Maestral

[Installation Guidelines](https://maestral.app/docs/installation)

Setup the environment

```zsh
python3 -m venv .maestral-venv
source .maestral-venv/bin/activate
```

install with

```zsh python3 -m pip install --upgrade 'maestral[gui]'```

launch: ```zsh maestral gui```

on error install:

```zsh sudo apt install libxcb-cursor0```

then

```zsh
/home/nikolask/.maestral-venv/bin/maestral_qt --config-name maestral
```
