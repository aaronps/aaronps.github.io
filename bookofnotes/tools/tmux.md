# tmux

tmux attach -> reattach

`ctrl-b` for tmux commands

* `d` - detach

* `:` - enter temux command prompt

Command promp commands:

```
join-pane -s <from_pane> -t <to_pane>
join-pane -s <from_pane>
join-pane -t <to_pane>

from_pane/to_pane: number or @number?

# enable mouse functions
set -g mouse-select-pane on
set -g mouse-resize-pane on



```
