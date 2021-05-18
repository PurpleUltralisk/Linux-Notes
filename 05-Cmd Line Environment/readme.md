# 05 - Command-line Environment

## Job Control

`ctrl c` stops execution of a process
The terminal/shell sends a signal `SIGINT` (signal interrupt) to tell the program to stop.
To see a list of all signals: `man signal`

To use signals with Python, we can import the `signal` library
The demo program handles SIGINT signal, and does not stop process.
But if we send a SIGQUIT with `ctrl \` then it stops.
This example goes to show that we can write handlers for these signals. If we want to save the state of our program when someone presses `ctrl c`, we can do that.

There are also signals that can't be handled: `SIGKILL`
`SIGKILL` also does not kill child processes.

`SIGHUP` is the hang up signal when we disconnect from ssh

`ctrl z` suspends a process by sending a `SIGSTOP` signal.

`nohup sleep 2000 &` - the `&` starts the program in the background
`jobs` shows the current jobs that are running.
To continue the job that was running, `bg %1` the 1 refers the the job # from running the `jobs` list.
`bg` is to recover a suspended process in the background.
`fg` is to recover a process to the foreground.
`kill -STOP %1` - allow us to kill the process
`kill -HUP %1` - sends the hangup signal can also stop the process
`kill -KILL %1` - sends the kill signal

## Terminal Multiplexers

`tmux` allow you to create more work spaces, like opening windows.

3 hiearchies:

1. Sessions
2. Windows
3. Panes

#### Sessions

To start a session: `tmux`
`ctrl a d` to detach from the session, or `ctrl b d` - most people remapped ctrl b with ctrl a
`tmux a` to reattached the session

`tmux new -t foobar` - start a session called foobar
`tmx ls` - to show a list of sessions
`tmux a -t foobar` - to reattach to the foobar session

#### Windows

Inside a session, you can have windows.
`ctrl a c` to create a new window
`ctrl a p` to jump back to previous window
`ctrl a n` to jump to next window
We can also use numbers to desginate which window we want to jump to
`ctrl a 1` to jump to 1st window
`ctrl a ,` to rename a window

#### Panes

`ctrl a "` horizontal split
`ctrl a %` vertical split

To move between panes: `ctrl a` + direction
`ctrl a space` equispace the current panes

`ctrl a z` zoom into a pane, make it take the entire screen
`ctrl a z` again to go back to previous view
You can also move panes

## Dot files

Configure shell to your workflow

#### Alias

It is used to remap longer commands
`alias ll="ls -lah"` - maps `ll` to `ls -lah`
Note: alias takes only 1 arugment, so no spaces in between

Also useful to give cmd a default flag
`alias mv="mv -i"`

To check existing alias
`alias ll`

How do we persist aliases?
For bash, we can edit the bashrc `vim ~/.bashrc`
`bash` - to start bash

You can also edit what information is presented in the terminal line
same concept for `~/.vimrc`

#### Symlinks

Create a folder to house all the dotfiles such as `.vimrc` `.bashrc`
Then any changes you make to the actual files will be forwarded to the files in this folder.

`ln -s` path-to-file symlink - to specify a symbolic link
Instead of the system reading `~/.bashrc` it will go look for it in your folder.

## Working with Remote Machines

`ssh jjgo@192.168.246.142`
jjgo is the user on the remote machine
@ separates the user from the address of the remote machine
This will prompt a password.
SSH is forwarding the session to our computer.

`logout` - will close the session

`ssh jjgo@192.168.246.142 ls -la` - will execute the command remotely

`ssh-keygen -o -a 100 -t eed25519` - to create a ssh key
`ls -la ~/.ssh/id*` - to list ssh keys
`cat ~/.ssh/id_ed25519.pub`- shows the text file

We copy this file to the server
`cat ~/.ssh/id_ed25519.pub | ssh jjgo@192.168.246.142 tee .ssh/authorized_keys`
`ssh-copy-id jjgo@192.168.246.142` is a shorter alternative

To copy files
`scp notes.md jjgo@192.168.246.142:notes.md`
The `:` separates the path

To copy lots of files
`rsync -avP . jjgo@192.168.246.142:cmd`

-   These flags check if files is already copied, preserve permissions if possible.
-   rsync will resume from where internet connection was interrupted, unlink scp

To edit ssh dotfile
`vim ~/.ssh/config`
Instead of typing the long username + address, we can persist it here

```ssh
Host vm
    User jjgo
    HostName 192.168.246.142
    IdentityFile ~/.ssh/id_ed25519
    RemoteFoward 9999 localhost:8888
```

1. `vim config` - edit our config
2. `cp config ~/.ssh` - copy our config to .ssh folder

Now we can just do `ssh vm` to connect to our remote machine

tmux config file is `vim .tmux.conf`
tmux also try to prevent you from opening another tmux
tmux into a ssh into another tmux -> have to type `ctrl a` twice
