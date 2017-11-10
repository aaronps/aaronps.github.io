# Erlang

## Console commands

* `help()` -> shows help
* `m(Module)` -> shows module info
* `code:delete(Module), code:purge(Module)` -> deletes module
* `{ok, Mod, Bin} = code:compile(FileName, [binary, MoreOptions])`
* `code:load_binary(ModuleName, UnusedFileName, BinaryCode).`
* `f()` -> forget all
* `f(X)` -> forget X

## notes

Shorter .cmd file to run `escripts`:
```cmd
	@escript %~dpn0.erl %*
```

Some bugs:

* "escript" with zip file cannot run, returns error about utf8, if change encoding to latin1, returns source code errors, so it is not trying to run, but to compile source code -> fail
* Adding emulator args to the archive makes escript unrunnable, it complains: cannot translate from UTF-8.
* Adding only a comment is ok, but on extract comment is undefined
* adding default shebang and comment, on extract is ok.
* "escript" documentation is wrong, parameter "-smp disable" is not supported, use "+S 1" instead
* NOTIFY ESCRIPT CHANGE: if it doens't start with a unix shebang it cannot contain comments or parameters, implore to allow to work as before.


## Standard libraries and functions

`erlang:element(N,Tuple)` -> returns the N (1..len) element of tuple
