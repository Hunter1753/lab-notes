## systemd user units
### random wallpaper each hour using hydrapaper
using [hydrapaper](https://aur.archlinux.org/packages/hydrapaper)

in ~/.config/systemd/user:

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
