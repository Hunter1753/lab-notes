# Tools

## temporary ftp server
`busybox tcpsvd -vE 0.0.0.0 2121 ftpd -A .`

## temporary sftp server
```bash
podman run \
    -v <localpath>:/home/foo/upload \
    -p 2222:22 -d docker.io/atmoz/sftp \
    foo:pass:1000
```
either match username and uid or `chmod 777` the local folder and `chown` all the files afterwards

## Route Audio To Multiple Sinks

use `sonusmix`
