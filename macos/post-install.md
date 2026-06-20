# macOS post installation notes

## System tweaks

### Screenshots

disable shadows
 
 ```defaults write com.apple.screencapture disable-shadow -bool true```

 save screenshot to jpg:
 
 ```defaults write com.apple.screencapture type  JPG```

at the end type:

```killall SystemUIServer```

### open unsigned / quarantined apps

- check where the application's path is:
```mdfind -name app```
- remove quarantine:
```sudo xattr -r -d com.apple.quarantine /Applications/yourapp.app```
