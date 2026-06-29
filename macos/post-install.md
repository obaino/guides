# macOS post installation notes

## System tweaks

### Screenshots

disable shadows
 
 ```defaults write com.apple.screencapture disable-shadow -bool true```

 save screenshot to jpg:
 
 ```defaults write com.apple.screencapture type  JPG```

at the end type:

```killall SystemUIServer```

### Snappier Dock

[reference](https://medium.com/geekculture/making-the-dock-snappier-6537a6214ad)

to implement:

```zsh
defaults write com.apple.dock autohide-delay -int 0
defaults write com.apple.dock autohide-time-modifier -float 0.4
killall Dock
```

to restore:

```zsh
defaults delete com.apple.dock autohide-delay
defaults delete com.apple.dock autohide-time-modifier
killall Dock
```

### open unsigned / quarantined apps

- check where the application's path is:
```mdfind -name app```
- remove quarantine:
```sudo xattr -r -d com.apple.quarantine /Applications/yourapp.app```

## Applications

### [Maccy](https://maccy.app/) / clipboard manager

### [stats](https://github.com/exelban/stats) / menu system info

### [maestral](https://maestral.app/) / dropbox alternative

### [XLD](https://tmkk.undo.jp/xld/index_e.html) / Lossless audio decoder for Mac OS

```zsh
xld -c cuefile.cue flacfile.flac -f mp3
```

convert Flac files in a folder to mp3 ver:

```zsh
for f in *.flac; do ffmpeg -i "$f" -q:a 0 "${f%.flac}.mp3"; done
```
