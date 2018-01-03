# Linux


quick and dirty sshfs, install `sshfs`

    mount -t fuse.sshfs user@ip:/path /mount/point

* ntp: `systemd ? timedatectl : ntpd`


## Development

See needed libraries and rpath data
    
    readelf -d <filename>

* if change `ld.so.conf` need to run `ldconfig`

* to escape `$ORIGIN` in one `Makefile` calling another `Makefile` use `-Wl,-rpath,'\$$\$$$$ORIGIN/../lib'`