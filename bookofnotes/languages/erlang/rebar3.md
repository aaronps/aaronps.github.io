# Rebar3

https://github.com/erlang/rebar3 can download from releases

Global configuration file; `~/.config/rebar3/rebar.config`
	
Config example with proxy
```erlang
	{plugins,[rebar3_auto]}.
	{http_proxy, "http://127.0.0.1:20088"}.
	{https_proxy, "http://127.0.0.1:20088"}.
```

Add to HOSTS if don't want to use proxy:
```
	151.101.198.2 repo.hex.pm
```

## commands

```shell
# rebar3 new <template_name> <project_name>
# rebar3 new -> list plugins
```

Rebar3 on the test suite has a rebar_localfs_resource which is what I wanted to use, I will make a plugin which makes the same (learned from it how to use rebar_file_utils:cp_r) and later make a localplugins plugin which will compile a set of files on a plugins folder and run its functions, the idea is that one file is one plugin.

### Escriptize

Will load all files on the ./ebin folder and add it to the $app/ebin folder, the problem is, if you have multiple apps, will it add to multiple of them?

## Notes

From `$project_base/_build/$profile/lib/*/ebin` only beams and app file will be copied to final destination
From `$project_base/ebin` all files will be copied to final destination

# To research

* What is the difference between plugins and project_plugins?
* project plugins
	Projects and dependencies might use the same local plugin but with different versions
	When compile, use a different module_name for local plugins, as to avoid clash.

# What's is this

_This section contains notes which I do not recal the meaning._

Provider hooks (as defined when providers:create/1 is called, are not used by rebar.


