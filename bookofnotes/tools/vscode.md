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