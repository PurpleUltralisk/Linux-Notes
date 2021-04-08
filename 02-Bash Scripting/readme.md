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
`/dev/null` is a directory, where whatever we write will be discarded
`2>`
`-ne` - comparison operator for not equal, for more info `man test`
If file did not have foobar, output != 0
Then appends foobar to end of file. 

To run it: `./example.sh mcd.sh script.py example.sh`
Then check `mcd.sh` to see `# foobar` added to end of file.

Globbing - or regex in commands
`ls *.sh` - lists all files with `.sh`

Expanding to line above 
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
`#!/usr/local/bin/python` This comment makes it possible to be executable using `./script.py a b c`. 
It tells the shell that it has to use python to execute the script. 

To make it able to run in every system change to `#!/usr/bin/env python`.
python is given as an arguement.



