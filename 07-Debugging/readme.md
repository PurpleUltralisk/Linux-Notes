# 07-Debugging and Profiling

printf debugging is quick and simple
logging - logs when some events happen

-   we can define severity levels, and filter
-   you can designate colors on your logs to terminal
-   logs are usually in `ls /var/log`

We can execute the program up to the moment it crashes:
`python -m ipdb bubble.py`

-   runs this program in python debugger
-   `l` is to list
-   `s` is to step
-   `restart` - restart the program -`c` - continue - to the error -`p arr` - print value of arr
-   `q` - quit

-`b 6` - set break point on the 6th line -`c` to continue to break point -`p locals()` - to print values of all contexts

## Other tools

`gdb` - works well with C, but works with any binary

-   displays very low level info

`strace`
`sudo strace ls -l > /dev/null`

-   see all the system calls that a program executed

`pyflakes` and `mypy`- static analysis tool

-   basically doesn't wait for the program to run to figure what's wrong with it

## Profiling

To optimize the code.
real time - entire time from start to finish
user time - time program spent on the CPU doing user level cycles
system time - time program spent on CPU doing kernel level cycles
`time curl https://....` - this will give a breakdown of the time spent

Profilers using comes in 2 flavors:
**Tracing** - everytime code enters a function call, it takes note of it.
Then it can provide a breakdown of how much time is spent on each function
But this adds alot of overhead.

**Sampling** - execute program, every defined period (100ms) it will stop the program and look at the stack trace.

Tracing example:
`python -m cProfile -s tottime grep.py 1000 '^(import|\s*def)[^,]$' *.py`
`s tottime` - sort by total time
`grep.py` - the program we are testing
`1000` - execute 1000 times
Rest is regex input to match all python files in folder.
But when it's calling 3rd party libraries, it's hard to tell what the code is doing.

`kernprof -l -v ulr.py`
This is a line profiler - for this line of code, it took this amount of time.

There are also memory profilers:
`python -m memory_profiler mem.py`

Check for outside events: how many cpu cycles, how many pagefaults?
`sudo perf stat stress -c 1`
`sudo perf record stress -c 1`
`sudo perf report`

## Other tools

Flame Graph - maps time used and stack
Call Graph - shows a graph of function calls
`htop` - interactive snapshots of CPU, memory info
`du -h` - disk usage
`ncdu` - interactive disk usage
`sudo lsof | grep ":4444 .LISTEN"` - check network usage
`hyperfine --warmup 3 'fd -e jpg' 'find . -iname "\*.jpg"' - benchmark 2 programs

More tools are listed on lecture tools
