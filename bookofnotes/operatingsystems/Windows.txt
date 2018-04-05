# Windows

## Notes

```cmd
rem Disconnect network drive
net use drive: /delete

```

## symlinks

* file link: `mklink filename path/to/link/target`
* softlink: `mklink /D dirname path/to/link/target`
* hardlink: `mklink /H filename path/to/link/target`
* junction: `mklink /J filename path/to/link/target`

## cmd scripts

```cmd 
rem `echo` multiple lines and pipe output
C:\> (echo line one & echo line two) | otherprogram
```

## VIM

Set default language to english and utf8: write on ~/.vimrc

```
set encoding=utf-8
set langmenu=en_US.UTF-8
language english
```