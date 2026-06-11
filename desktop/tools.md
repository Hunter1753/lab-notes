# Tools

## temporary ftp server
`busybox tcpsvd -vE 0.0.0.0 2121 ftpd -A .`

## temporary sftp server
```bash
rclone serve sftp . \         
  --user tempuser \
  --pass temppassword \
  --addr :2222
```
either match username and uid or `chmod 777` the local folder and `chown` all the files afterwards

## Route Audio To Multiple Sinks

use `sonusmix`
