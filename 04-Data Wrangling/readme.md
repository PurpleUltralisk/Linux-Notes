# 04 - Data Wrangling

`journalctl` command produces system log
We will filter some data using `grep` command: `ssh tsp journalctl | grep ssh`

But we're getting the entire log from the server, then running grep.
It would be more efficient if we filter the data on the server before sending it back to us.
