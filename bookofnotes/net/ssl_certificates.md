# SSL certificates

## letsencrypt

1. install `certbot` (in centos: epel)
2. if have webserver, find its root then `certbot certonly --webroot -w /path/to/webroot -d domain.name domain2.name`
3. if no webserver: `certbot certonly --standalone -d domain.name -d domainx.name`
4. after finish, it will tell you where are the files

### renew letsencryp

Because certs are 90 days, you have to renew

`certbot renew`

## self signed certificate authority

1. without passwortd: `openssl genrsa -out rootca.key 2048`
1. with password: `openssl genrsa -des3 -out rootca.key 2048`
2. self sign ca: `openssl req -x509 -new -nodes -key rootca.key -sha256 -days 1024 -out rootca.pem`
    * this will ask some questions

## self signed normal certificate

1. `openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout somename.key -out somename.crt`

another way

1. `openssl genrsa -out filename.key 2048`
2. `openssl req -new -x509 -key filename.key -out filename.cert -days 365 -subj /CN=alia.aaronps.com`
