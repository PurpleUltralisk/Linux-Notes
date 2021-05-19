# 08 - Metaprogramming

Process around how you develop software.

## Build System

There's a sequence of commands you need to run.
Build system combines this sequences.
`npm` is an example of a build system.

Targets - things we want to build
Dependencies - things that needs to be built first
Rules - how do we go from dependencies to the target

`make` works well for simple to medium sized projects
`make` will look a file called `makefile` in the current dir.
`vim Makefile`

```
paper.pdf: paper.tex plot-data.png
    pdflatex paper.tex

plot-%.png: %.dat plot.py
    ./plot.py -i $*.dat -o $@
```

paper.pdf is dependent on paper.text and plot-data.png
If either of these two dependencies change, build paper.pdf
.tex is a programming language for documents
`%` is a wildcard pattern for any string
plot-%.png depends on the data file
`./plot.py` runs the python file, `-i` marks the input
`$*` - special makefile variable that matches what the `%` was
ie. `plot-foo.png` will look for `foo.dat`, and `$*` will match with `foo`
`$@` - special variable for name of the target - ie. the output file

## Symantic Versioning

Major version.minor version. patch version
patch verison - Change is backwards compatible
minor verison - Add something to library
major version - Backward incompatible change

Lockfile - make sure we don't accidentally update something

-   speeds up makefile
-   ensure dependency

Vendoring - take the dependency and copy it into project

## Continuous Integration

Event invokes a service
You write a file to indicate which event you want to monitor

Dependabot - indicate when your dependencies are out of date
Github pages - lets you set up CI action, builds your repositor as a blog

-   Runs a static site generator `jykell`, produces a complete website
    Github push -> jykell pulls all markdown pages and make into html -> Github pages

## Testing

Test suite - all of the tests in a program
Unit test - small test, self contained, tests a single feature
Integration test - test interactions of sub systems
Regression test - test things that were broken in the past, to prevent that bug from happening again

## Mocking

Being able to replace parts of a system with a dummy system, to behave in a controlled manner.
ie. abstract server

## Build

Google has `basel` which is a make software for entire ecosystem
`maven` - make for Java
`cmake` - make for c
