# Using Timeshift on Ubuntu 20.04

Timeshift is a really good tool to restore system settings, for when we mess up some configurations on our machine.

## Installation

`$ sudo apt install timeshift`

We can open up timeshift from our search.

## Setup Backup Options

If the wizard does not popup to ask you which snapshot type you want, you can open it mannually by clicking on the `wizard` button.
We will use the `rsync` option.
From there, the options are pretty straightfoward.
The only option we have to make a decision on is whether or not you want to include all files or not.

## Create a back up

Just click on `create` button to create a new snapshot

## Restore

To restore to a previous snapshot, just select the snapshot you want and click on the `restore` button.
