# How my desktop pc is set up

## os + de
EndeavourOS + Gnome

# gnome shell look
- marble shell theme

### gnome extensions
- https://github.com/ubuntu/gnome-shell-extension-appindicator
- https://github.com/aunetx/blur-my-shell
- https://micheleg.github.io/dash-to-dock/
- https://github.com/sidevesh/solaar-extension-for-gnome
- https://extensions.gnome.org/extension/6807/system-monitor/
- https://extensions.gnome.org/extension/19/user-themes/
- https://github.com/nunofarruca/WindowIsReady_Remover

### terminal
- font: SauceCodePro Nerd Font Mono
- shell: zsh + ohmyzsh + p10k
- 

## keyboard + mouse
solaar with extension for gnome

rules.yaml
```yaml
%YAML 1.3
---
- Rule:
  - Rule:
    - Key: [Mute Microphone, pressed]
    - KeyPress:
      - XF86_AudioMicMute
      - depress
  - Rule:
    - Key: [Mute Microphone, released]
    - KeyPress:
      - XF86_AudioMicMute
      - release
- Rule:
  - Rule:
    - Key: [Calculator, pressed]
    - KeyPress:
      - XF86_AudioMute
      - depress
  - Rule:
    - Key: [Calculator, released]
    - KeyPress:
      - XF86_AudioMute
      - release
- Rule:
  - Rule:
    - Key: [Show Desktop, pressed]
    - KeyPress:
      - XF86_AudioPrev
      - depress
  - Rule:
    - Key: [Show Desktop, released]
    - KeyPress:
      - XF86_AudioPrev
      - release
- Rule:
  - Rule:
    - Key: [MultiPlatform Search, pressed]
    - KeyPress:
      - XF86_AudioPause
      - depress
  - Rule:
    - Key: [MultiPlatform Search, released]
    - KeyPress:
      - XF86_AudioPause
      - release
- Rule:
  - Rule:
    - Key: [Lock PC, pressed]
    - KeyPress:
      - XF86_AudioNext
      - depress
  - Rule:
    - Key: [Lock PC, released]
    - KeyPress:
      - XF86_AudioNext
      - release
...
```

solaar.desktop in `~/.config/autostart`
```
[Desktop Entry]
Name=Solaar
Comment=Logitech Unifying Receiver peripherals manager
Comment[fr]=Gestionnaire de périphériques pour les récepteurs Logitech Unifying
Comment[hr]=Upravitelj Logitechovih uređaja povezanih putem Unifying i Nano prijemnika
Comment[ru]=Управление приёмником Logitech Unifying Receiver
Comment[es]=Administrador de periféricos de Logitech Receptor Unifying
Exec=solaar
Icon=solaar
StartupNotify=true
Terminal=false
Type=Application
Keywords=logitech;unifying;receiver;mouse;keyboard;
Categories=Utility;GTK;
#Categories=Utility;GTK;Settings;HardwareSettings;
X-GNOME-UsesNotifications=true
```

## systemd user units
### random wallpaper each hour using hydrapaper
using [hydrapaper](https://aur.archlinux.org/packages/hydrapaper)

in `~/.config/systemd/user`:

random_wallpaper.service
```
[Unit]
Description="Sets random wallpaper using HydraPaper"

[Service]
ExecStart=/home/aguttrof/.config/systemd/user/random_wallpaper.sh
```

random_wallpaper.timer
```
[Unit]
Description="Run random_wallpaper.service 5min after login and every hour relative to activation time"

[Timer]
OnBootSec=5min
OnUnitActiveSec=1h
Unit=random_wallpaper.service

[Install]
WantedBy=default.target
```

random_wallpaper.sh
```
#!/bin/zsh
hydrapaper -r
```

enable with `systemctl --user enable random_wallpaper.timer`
