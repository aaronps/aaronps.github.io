# vscode

## Tasks

Select menu `Tasks -> Configure Tasks`

To run: `Ctrl+P` then run task name

To hide asking what to do with output, go configure tasks: `"problemMatcher": []`

For setting keys to run: (use intellisense ctrl+space to expand things) 

* Menu `File -> Preferences -> Keyboard Shortcuts` or `ctrl+k ctrl+s`
* edit `keybindings.json`
```json
    { "key": "ctrl+xxx", "command": "runTask", "args": "taskname"}
```
	
