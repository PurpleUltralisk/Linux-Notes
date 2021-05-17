# 04 - Data Wrangling

`journalctl` command produces system log
We will filter some data using `grep` command: `ssh tsp journalctl | grep ssh`
`tsp` here is the name of the server.

## Filter for data on server

But we're getting the entire log from the server, then running grep.
It would be more efficient if we filter the data on the server before sending it back to us.
We can do this by adding `'` to our command like so:
`ssh tsp 'journalctl | grep --line-buffer ssh | grep --line-buffer "Disconnected from"' | less`

`less` is a pager, basically takes really long content and fit it into pages.

## Output Data

This command will take the output and put it into a log file
`ssh tsp 'journalctl | grep ssh | grep "Disconnected from"' > ssh.log`

We can view this file with `cat ssh.log | less`

## sed and Regex

[regex101.com](regex101.com) is a good tool to check regex matching.

`sed` is a stream editor, make changes to content of the stream.
Most common thing to do is just to replace words.
`cat ssh.log | sed 's/.*Disconnected from //'`
Here we are using `sed` expressions:
s - is the substitute expression, it takes 2 arguments enclosed in the `/`s
`.` - is regex for any single character
`.*` - `*` following a character is 0 or more of the previous character. Here, it means 0 or more of any character.

`[]` - is regex to match any character specified in the `[]`
` echo 'aba' | sed 's/[ab]//'` - will output `ba`
` echo 'bba' | sed 's/[ab]//'` - will output `ba`
Note that this pattern is only matched once, by default.
To find all matches, we can give it the `g` modifier.
` echo 'aba' | sed 's/[ab]//g'` - will output nothing

`()` - is regex for a exact pattern
` echo 'abcaba' | sed -E 's/(ab)*//g'` - will output `ca`
Here any occurrences of `ab` together will be replaced with nothing.
**Note:** We should run `sed` with `-E` modifier, because `sed` is a really old tool.
Or we can get the same result with this:
` echo 'abcaba' | sed 's/\(ab\)*//g'` - will output `ca`
The `\` tells sed to use the special meaning of the brackets.

`|` - is regex for OR
` echo 'abcaba' | sed -E 's/(ab|bc)*//g'` - will output `cc`
Will replace `ab` or `bc`.

Regex is inherently greedy.
`echo 'Disconnected from invalid user Disconnected from 84.211' | sed 's/.*Disconnected from //'`This will remove from the 2nd occurrence of Disconnected from.

To match outputs like these:

```
Jan 10 23:09:08 thesquareplanet.com sshd[20215]: Disconnected from user jon 24.61.9.143 port 51098
Jan 10 23:09:08 thesquareplanet.com sshd[20215]: Disconnected from invalid user admin 58.120.227.29 port 52978 [preauth]
```

`cat ssh.log | sed -E 's/^.*Disconnected from (invalid )?user .* [0-9.]+ port [0-9]+( \[preauth\])?$//'`
`?` - is regex for 0 or 1 occurrence
`+` - is regex for 1 or more occurrences
`^` - match beginning of a line
`$` - matches the end of a line

#### Capture Groups

Anything inside `()` is a capture group.
`cat ssh.log | sed -E 's/^.*Disconnected from (invalid )?user (.*) .* [0-9.]+ port [0-9]+( \[preauth\])?$/\2/'`
Here `(invalid )` is a capture group, and we are looking for 1+ instance of it.

We want to capture the usernames, so here `user (.*)` does not have any modifiers.
It will match 1 time.
We can then refer back to the capture group by using `\2`. Here we replace the entire string with the username that we got matched with in the 2nd capture group.
