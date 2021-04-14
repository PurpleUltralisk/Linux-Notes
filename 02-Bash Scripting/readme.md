# 02-Bash Scripting 

Define a variable: `foo=bar`
Now foo is set: `echo $foo`
Notice that the spacing is very sensitive. 

To reference foo in a string, use double quotes: `echo "value of foo is $foo"`
Single quotes turns it to a string literal.

Setting up a simple function: 
`vim mcd.sh`

```
mcd () {
    mkdir -p "$1"
    cd "$1"
}
```
Here we are calling `mkdir -p` function. 
Type `source mcd.sh` to load the script, now mcd is a recognized function. Try `mcd test`
`$1` - represents the 1st argument 
`$0` - name of the script
`$2` to `$9` - other arguments

`$_` - last argument of the previous command
`!!` - run last command, use case `sudo !!`

`$?` - returns the value output of last command. 
Outputs 0 if program ran successfuly, 1 if errors occurred. 
```bash
echo "Hello"
echo $?

grep foobar mcd.sh
echo $? 
```

To get output of a cmd into a variable: 
```bash
foo=$(pwd)
echo $foo
echo "Current directory is $(pwd)"
```

`cat <(ls) <(ls ..)`
`ls ..` executes to list parent directory
`ls` executes to list files in current directory
Both outputs are concatenated. 

Example bash script
```bash
for file in "$@"; do
    grep foobar "$file" > /dev/null 2> /dev/null
    
    if [["$?" -ne 0]]; then
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file"
    fi
done
```
`$#` - Number of arguments 
`$$` - pid, process id 
`$@` - expand to all the arguments
`grep` search for a substring
`/dev/null` is a notional directory, whatever we write to this folder will be discarded
`2>` this is equivalent to else statement

Then we check the output of the `grep` command: 
`-ne` - comparison operator for not equal, for more info `man test`
If file did not have foobar, output != 0
Then appends foobar to end of file. 

To run it: `./example.sh mcd.sh script.py example.sh`
Then check `mcd.sh` to see `# foobar` added to end of file.

## Globbing - or regex in commands
`ls *.sh` - lists all files with `.sh`

## Expanding to line above  
```bash
convert image.png image.jpg
convert image.{png,jpg}

cp project{1,2}
touch project{1,2}/src/test{1,2,3}.py

touch {foo,bar}/{a..j} 
```

We can also do this with other languages: 
Python example
```python
#!/usr/local/bin/python
import sys
for arg in reversed(sys.argv[1:]):
    print(arg)
```
Normally, you'd call the function using `python script.py a b c`
`#!/usr/local/bin/python` the shell that it has to use `python` to execute the script. 
This comment makes it possible to be executable using `./script.py a b c`. 

To make it able to run in every system change to `#!/usr/bin/env python`.
Run the python environment variable. 

## Error Checking Bash Scripts
use `shellcheck mcd.sh`

## Manual / TLDR
`man <command>` shows the manual for the commmand
`tldr <command>` shows a shorter version 

## Find Files
`find . -name src -type d` 
Starting in current folder `.`, find directory (`-type d`) with name src (`-name src`). 

`find . -path "**/test/*.py" -type f` 
Any number of folders, where there's a `test` folder housing a `.py` file.
`-type f` stands for file. 
`-mtime -1` last modified yesterday

We can also do stuff with the files found. 
`find . -name "*.tmp" -exec rm {} \;`
Execute `rm` command for every file found. 

## Locate
Looks for paths in the filesystem with the argument
`locate folderName`

`updatedb` updates this filesystem database

## Recursive grep
`grep -R wordtoFind` will go through all subdirectories. 

or use `rg` command this is called `ripgrep`. 
`rg` "import requests" -t py ~/scratch`
Or add 5 lines of contexts
`rg` "import requests" -t -C 5 py ~/scratch`

`rg -u --files-without-match "^#\!" -t sh`
`-u` don't ignore hidden files
This command asks to print all the files that don't match with the parameter given. 
`"^#\!` - search files that don't have a `#` at the beginning. 

Other similar tools: 
`ag`
`ack`

## Finding old commands
`history` will print all old commands
Then we can try to find the ones we want 
`history 1 | grep convert` - commands containing `convert`

`ctrl+r` is a backward search, this is the same as the previous command.

## FuzzyFinder fzf
`cat example.sh | fzf`
Then we can interactively look for the string 
Default binding is `ctrl+r`. 

## History substring search
Dynamically search command history as you type
Right arrow selects the command. 

## tree
Prints directory 

## broot
Same as tree, but has an 'unlisted' to collapse the files.
Also has fuzzymatching

## nnn
interactive 



