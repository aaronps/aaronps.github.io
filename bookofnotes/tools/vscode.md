# vscode

## keys

```
Keyboard shrtcuts
    ctrl-k ctrl-s

duplicate line or selected lines:
    alt-shift-(up or down)

move line or selection up/down:
    alt-(up or down)

run build task:
    ctrl-shift-b

run task:
    ctrl-shift-p
        search for run task
        select task name

intellisense:
    ctrl-space

zoom-in/out:
    ctrl-+ / ctrl--

select theme:
    ctrl+k ctrl-t
    

```

## Tasks

Tasks uses a configuration file: `.vscode/tasks.json` on the current opened
folder. You can create yourself, or select menu `Tasks -> Configure Tasks`. When
editing the file, use intellisense (`ctrl+space`) to know what to type.

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "group": "build", // will appear on ctrl-shift-b menu
            "label": "task name",
            "type": "shell",
            "command": "command to run",
            "presentation": {
                "panel": "dedicated" // default is shared 
            },
            "problemMatcher": [] // avoid vscode asking you what to do with output

        }
    ]
}
```

For setting keys to run tasks (_use intellisense, luke_) 

* Menu `File -> Preferences -> Keyboard Shortcuts` or `ctrl+k ctrl+s`
* edit `keybindings.json`
```json
{ "key": "ctrl+xxx", "command": "runTask", "args": "taskname"}
```
	
## plugins selection

vuejs
* vetur by Pine Wu

java
* Java Extension Pack by Microsoft 
* Gradle lanuage support by Naco Siren

c++
* C/C++ by Microsoft
* CMake by twxs
* CMake Tools by vector-of-bool

themes
* Darcula theme by rokoroku

## Settings

_ctrl-, to edit_

```json
{
    // this is changed automatically when using ctrl-+ and ctrl--
    "window.zoomLevel": 0,
    
    // change default terminal to cmd (or other)
    "terminal.integrated.shell.windows": "C:\\WINDOWS\\System32\\cmd.exe",
    
    // add vertical lines (multiple)
    "editor.rulers": [
        80
    ],
    
    // config java when using java plugins
    "java.home": "C:\\Program Files\\Java\\jdk1.8.0_151",

    // disable minimap
    "editor.minimap.enabled": false
}
```