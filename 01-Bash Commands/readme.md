# 01-Bash Commands

| Command | Description | Examples |
| --- | --- | --- |
| date | Display current date | date |
| echo <arg> | Print argument | echo "Hello" |
| \ | Escape character | echo Hello\ World | 
| $variable | Environment variable | echo $PATH | 
| which <arg> | Which program would run | which echo | 
| pwd | Print working directory, print current path | pwd |
| cd <arg> | Change directory, `.` is current dir `..` is parent dir| cd /abspath/folder , cd ../../../another folder/subfolder |
| cd ~ | ~ is always home directory | cd ~ |
| cd - | - is directory previously in | cd - |
| ls | list all folders | ls |
| mv <arg1> <arg2> | Move or rename a file | |
| cp <arg1> <arg2> | Copy file to destination |  |
| rm <arg1> | remove file/path |  |
| rm -r <arg1> | recursively remove file/path |  |
| mkdir <arg1> | Make directory | |
| man <cmd> | Show manual for a program | man ls |
| > | Output stream | echo hello > hello.text |
| cat | Print content of file | cat hello.txt |
| < | Input stream, set input of program | cat < hello.text , set input to be hello.text |
| < | Input stream, set input of program | cat < hello.text > hello2.text, similar to `cp` |
| >> | Output stream, append to end | cat < hello.txt > hello2.text |
| >> | Output stream, append to end | cat < hello.txt > hello2.text |
| tail -n1 | print last n lines | |
| `\|` | Pipe, take output of prog1 as input for prog2 | ls -l / | tail -n1 |
| cd sys | System parameters that looks like a file system | |
| sudo su | Run shell as super user | |
| tee <file> | Print output to console and to file | |
| xdg-op <file> | Opens file in appropriate program | |
| sudo su | Run shell as super user | |

### --help syntax 
`[ ]` brackets mean optional argument
`...` means one or more arguments


