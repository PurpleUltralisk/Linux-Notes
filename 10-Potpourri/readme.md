# 10 - Misc Topics

[Lecture 10](https://missing.csail.mit.edu/2020/potpourri/)

## Keyboard remappings

## Daemon processes

Programs in the background and waiting for events to happen.
These daemons usually ends with a d.
Ex. `sshd` listens for ssh connections

Uses `systemctl` to check status of these daemons
Computer expects daemons to be run using some `systemd` unit.
It's like a configuration that tells the system how to run the process.

You can write a daemon that does something periodically.
Linux and mac has `crond`

## File System

`sshfs` - do file operations on a remote server

## Backups

Try to set up back ups.

## APIs

Most services have APIs for you to interact with.
Usually just send a web request to the service, and get JSON data back.
`curl` - fetchs the url
`jq` - is a json query tool, get JSON input and allow you to query on that data

Some require authentication via Oauth tokens.
Don't push these tokens to Github.

`If this then that` is a webservice

## Command Line Arguments

`--help` - is a common flag
`--version` - to get version of the software
`-v` or `--verbose` - increase the output info, can also do `-vvvvv`
`-quiet` or `-silent` - not print output

Destructive tools like `mv` or `rm`
`dry run flag` - run the tool, but won't make any changes, but inform you of the changes
`-i` - interactive mode, prompts you for decision to go ahead or not
`-r` - recursive

How do you remove a file called `-i`?
`rm -i` is a rm command with a flag
To opt out of a flag use `--`, everything following `--` do not interpret as flag
Must have space on both sides of the `--`
For remove: `rm -- -i`

## Window Managers

Normally we are using `floating window manager`, what we are used to in Windows
`tiling window manager` is an alternative

-   Everything is set up in a tiling layout
-   new window takes up a subset of the total space
-   like tmux panes

## VPNs

Best case = change internet service provider

-   make traffic seem like its coming from somewhere else

HTTPS or TLS already encrypts the data.

Public wifi network traffic to router may not be encrypted.
People can see which DNS your going to.
You can encrypt your traffic via `DNS over TLS` or `DNS over HTTPS`
DNS - is how computer checks for ip addresses

## Markdowns

Convenient way to format texts

## Desktop Automation

`Hammerspoon` - desktop automation for MacOS, write lua scripts

-   connects to window management, display management, file system, everything OS manages
-   bind hotkeys to move windows to locations

## Booting and live USBs

Something happens in the boot process when the OS is loaded.
Useful if your desktop crashed, we can use the boot usb to mount the file system.

## Virtual Machines, Cloud, and Docker

Use VM or container for development environment
There are also tools that let you script hyperviors to get the VM you want.
`vagrant` is a good tool

## Notebook programming environment

Jupyter note book - interactive programming
But this runs code step-wise

## Github

Two wayts to contribute to open source projects:

1. Issues - report a bug
2. Pull requests - see guides
   Steps: fork project to get local copy -> code -> send pull request
