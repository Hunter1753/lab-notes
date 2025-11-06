# How my desktop pc is set up

## systemdboot
`/EFI/loader/loader.conf`: change the line beginning with `default` to `default @saved`

## librewolf
in `$HOME/.librewolf/librewolf.overrides.cfg`
```cfg
lockPref("privacy.resistFingerprinting.letterboxing", true);
lockPref("identity.fxaccounts.enabled", true);
lockPref("browser.sessionstore.resume_from_crash", false);
pref("webgl.disabled", true);
lockPref("privacy.sanitize.sanitizeOnShutdown", false);
lockPref("privacy.clearOnShutdown.cache", false);
lockPref("privacy.clearOnShutdown.cookies", false);
lockPref("privacy.clearOnShutdown.downloads", false);
lockPref("privacy.clearOnShutdown.formdata", false);
lockPref("privacy.clearOnShutdown.history", false);
lockPref("privacy.clearOnShutdown.offlineApps", false);
lockPref("privacy.clearOnShutdown.sessions", false);
lockPref("privacy.clearOnShutdown.siteSettings", false);
```

## os + de
EndeavourOS + Gnome

## gnome shell look
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
- shell: zsh + p10k
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
      - XF86_AudioPlay
      - depress
  - Rule:
    - Key: [MultiPlatform Search, released]
    - KeyPress:
      - XF86_AudioPlay
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

## systemd units
### solaar /dev/uinput rule
```
[Unit]
Description=Grant uinput access to a specific user
# Remove After=dev-uinput.device

[Service]
Type=oneshot
ExecStart=/usr/bin/bash -c 'if [ -e /dev/uinput ]; then setfacl -m u:YOURUSER:rw /dev/uinput; fi'
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
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

## rootless docker
- only works some of the time for other stuff
- works well with ipex-llm
  ```bash
  #!/bin/zsh
  docker run -d \
      --net=bridge \
      --device=/dev/dri \
      -p 11434:11434 \
      -v ./models:/root/.ollama/models \
      -e PATH=/llm/ollama:$PATH \
      -e OLLAMA_HOST=0.0.0.0 \
      -e no_proxy=localhost,127.0.0.1 \
      -e ZES_ENABLE_SYSMAN=1 \
      -e OLLAMA_INTEL_GPU=true \
      -e ONEAPI_DEVICE_SELECTOR=level_zero:0 \
      -e DEVICE=Arc \
      --shm-size="16g" \
      --memory="20G" \
      --name=ipex-llm \
      intelanalytics/ipex-llm-inference-cpp-xpu:oneapi_2024.2 \
      bash -c "cd /llm/scripts/ && source ipex-llm-init --gpu --device Arc && bash start-ollama.sh && tail -f /llm/ollama/ollama.log"
  ```

## rootless podman
- works well with devcontainers using vscode
  - In order to have the files listed as the USER declared in the Dockerfile, add the --userns=keep-id flag in the runArgs. as per https://github.com/microsoft/vscode-remote-release/issues/5296#issuecomment-875379969
- works well with distrobox for gui applications
- use for Affinity by Canva via lutris and winetricks [AffinityOnLinux](https://github.com/seapear/AffinityOnLinux)
- use for other apps to not pollute base system

## HardDrives

```
UUID=xxxx-xxxxx-xxxxx /mnt/kingston4 ext4    rw,relatime 0 2

# mounting entire BTRFS volume
UUID=xxxx-xxxxx-xxxxx /mnt/defvol    btrfs   subvol=/,rw,relatime,compress=lzo,discard,space_cache 0 0
```
