# npm

Config commands

	npm config list
	npm config edit
	npm config get
	npm config set

Config notes

	proxy=socks5://ip:port
	https-proxy=socks5://ip:port
	or
	proxy=http://ip:port/
	https-proxy=http://ip:port/
	
	git-tag-version=false

Default registry

	https://registry.npmjs.org/

Set Taobao registry for China

	npm config set registry https://registry.npm.taobao.org

Install 'cnpm' without proxy

	npm install -g cnpm --registry=https://registry.npm.taobao.org

To create a project

	npm init

Check for outdated dependencies

	npm outdated
	(for globally installed) npm -g outdated

For updating things

	npm update XXX
	npm -g update XXX

Check npm for errors

	npm cache verify

Preffer local data over internet check

	npm install --prefer-offline


To review

	npm pack -> generate a package file
	npm install (folder | package.tgz)
	npm link ( '' | package name )
	 