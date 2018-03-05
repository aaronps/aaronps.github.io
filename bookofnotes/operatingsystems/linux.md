# Linux

```bash
# mount by ssh
# 1- install sshfs
mount -t fuse.sshfs user@ip:/path /mount/point

# NTP sync
# by systemd
timedatectl --help

# by ntpd
ntpdate _server_

# view system version
lsb_release -a

```

## Centos

* If after long time of not using centos, `yum update` fails, use `yum clean all`.

## systemd

* if the **service** has autorecovery code, for example, you connecto to a JMS
server and you reconnect when connection drops. **USE `Wants` INSTEAD OF
`Requires`**

* If you want to start services in some order, use `After=` or `Before=`

Disable stdout -> journal, add to your .service file:
    
    StandardOutput=null

## Development

See needed libraries and rpath data:

    readelf -d <filename>

After modify `ld.so.conf`, run:

    ldconfig

### RPATH

Escape `$ORIGIN` in one `Makefile` calling another `Makefile`:

    -Wl,-rpath,'+++\$$\$$$$ORIGIN/../lib+++'

When using RPATH, if you want to use _not linked shared_ libraries to RPATH
you still need to link them, otherwise won't be searched on RPATH:
        
    g++ other/files -L ... -lotherlibs -L indirect/libraries/path -lindirectlib

