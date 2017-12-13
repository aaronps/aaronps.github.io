# Linux


quick and dirty sshfs, install `sshfs`


    mount -t fuse.sshfs user@ip:/path /mount/point

* ntp: `systemd ? timedatectl : ntpd`


## Development

See needed libraries and rpath data
    
    readelf -d <filename>

 