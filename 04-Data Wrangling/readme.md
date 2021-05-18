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

## wc

Word count
`wc -l` will count the number of lines

## sort | uniq

` | sort | uniq -c`
Sort the results, count only unique results.
` | sort | uniq -c | sort -nk1,1`
Sort again after the unique filter.
`-n` is numeric sort.
`k1,1` start in first column

## awk

Similar to `sed` as an editor, but focuses on the column of data.
`| sort | uniq -c | sort -nk1,1 | tail -n20 | awk '{print $2}' | paste -sd,`
`awk '{print $2}` - print the 2nd column
`paste` takes a bunch of lines combines it into a single line.
`-sd,` means single line, with delimiter being `,`

We can use regex with awk too:
` | sort | uniq -c | awk '$1 == 1 && $2 ~/^c.*e$/ {print $0}`
`$1 == 1` - first column be 1 time  
second column to match with the regular expression

## bc - a calculator

` echo "1 + 2" | bc -l`
We can pipe results using `awk` and `paste -sd+` to change it to an arithmetic expression.

## R - is also callable

gnuplot can plot data as well

## xargs

Takes lines of input and turns them into arguments
`rustup toolchain list | grep nightly | grep -v 'nightly-x86' | grep 2019 | sed 's/-x86.*//' | xargs rustup toolchain uninstall`
Here we format a list of Rust versions to find the ones we want to delete.
Passing it to xargs with the command rustup toolchain uninstall.

## ffmpeg and convert

`ffmpeg -loglevel panic -i /dev/video0 -frames 1 -f image2 - | convert - -colorspace gray - | gzip | ssh tsp 'gzip -d | tee copy.png' | feh -`

`ffmpeg` does encoding and decoding images.
`convert` can convert images
`tee` reads input, prints it to stdout and makes a copy
`feh` displays the image
