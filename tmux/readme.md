# Tmux Tutorial

To get into tmux: `$ tmux`
Open windows are labeled at the bottom. Current window is higlighted by `*`.

## Creating Panes

To split window into pane to right: `ctrl+b %`
To split pane to bottom: `ctrl+b "`
To move between panes: `ctrl+b left/right`

## Create Window

To create new window `ctrl+b c`.
This window should now be labeled at the bottom.

To switch between windows `ctrl+b <window number>`
To rename a window `ctrl+b ,` then enter name for window

## Sessions

tmux is great for persisting session state.
To list all the sessions `$ tmux ls`
To attach to a session `$ tmux attach -t <session_name/#>`
To detach from a session `ctrl+b d`

To rename a session `$ tmux rename-session -t <session_name/#> <new_name>`
To open a new session `$ tmux new -s <session_name>`
To kill a session `$ tmux kill-session -t <session_name>`
