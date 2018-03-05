# Erlang

console commands:

```erlang
%% Some help
help().

%% show module info
m(Module).

%% compile without loading
{ok, Mod, BinaryCode} = code:compile(FileName, [binary, MoreOptions]).

%% load binary module
code:load_binary(ModuleName, UnusedFileName, BinaryCode).

%% unload module 
code:delete(Module), code:purge(Module).

%% forget all bindings on the current console.
f().

%% forget X on the current console
f(X).
```

## Standard libraries and functions

```erlang
%% Get N element from tuple (starting from 1)
erlang:element(N,Tuple).

```

## notes

Shorter .cmd file to run `escripts`

	@escript %~dpn0.erl %*

## Known bugs

* "escript" with zip file cannot run, returns error about utf8, if change encoding to latin1, returns source code errors, so it is not trying to run, but to compile source code -> fail
* Adding emulator args to the archive makes escript unrunnable, it complains: cannot translate from UTF-8.
* Adding only a comment is ok, but on extract comment is undefined
* adding default shebang and comment, on extract is ok.
* "escript" documentation is wrong, parameter "-smp disable" is not supported, use "+S 1" instead
* NOTIFY ESCRIPT CHANGE: if it doens't start with a unix shebang it cannot contain comments or parameters, implore to allow to work as before.
