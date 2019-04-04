# Migrating Legacy Python 2 Scripts

Python 2 will not be maintained past 2020. Many scripts around the company rely on Python 2, and many should consider migrating to a supported version or another language.

# Why Should Scripts be Migrated?
The lack of support for Python 2 itself does not cause monumental problems. Many prominent Python packages are also dropping support for Python 2, however, and this is likely to cause problems. The list of packages that are dropping support is [huge](https://python3statement.org/), but some highlights include: TensorFlow, Pandas, SciPy, NumPy, MatPlotLib, pytest, and Jupyter. When adapting old scripts for use on a new project, new features of these libraries will inevitably become useful.

# Are there any exceptions?
Legacy embedded devices may only support Python 2. In these cases, it will be necessary to freeze all packages to versions that are compatible with Python 2. Additionally, if it is unlikely that a script will be used in future projects, it is probably not worth the effort to convert.

# What Replaces Python 2?
The obvious replacement to Python 2 is Python 3. Python 3 has been in release for over 10 years, and provides additional functionality over Python 2. Python 3 has good package support, good community support, and is widely used. One potentially misplaced concern with Python 3 has been that its performance on particular tasks has historically been far slower than Python 2. In particular, Python 3.0 had poor IO performance (see StackExchange post by core developer [here](https://softwareengineering.stackexchange.com/questions/63859/why-do-people-hesitate-to-use-python-3). As of the latest stable Python 3 release (3.7), performance concerns have been mitigated considerably. For a specific task breakdown of this, see this [hacker noon post](https://hackernoon.com/which-is-the-fastest-version-of-python-2ae7c61a6b2b). If the performance of Python 3 is a concern, then it is likely that Python 2 may also be too slow for your needs. One option is to try out [PyPy](https://pypy.org/), a JIT compiled version of Python which can be much faster than the traditional distributions. Another option that is increasingly viable is to migrate the project to Julia. The [Julia Language](https://julialang.org/) offers performance approaching C, while retaining the flexibility and generality of Python.

# Julia for Performance-Sensitive Tasks
The Julia language is, as the creators say, created for "greedy" programmers. The goal is to approach the speed of C, while allowing for the ease of use of Python or Matlab. In addition to this performance boost, Julia has several other advantages, such as the ability to directly call functions in c code, system commands and bash scripts and python scripts. This functionality makes Julia a great high-level scripting language, in addition to a fast low-level language. The ability to handle both of these uses in a single language can go a long way towards gluing together the heaps of c programs, shell scripts and python that hold most current projects together. Below are some examples of how Julia could simplify the stack on your next project.

# Julia Examples
## Calling C & Fortran Functions
While Julia is efficient and can be very fast, there is already a ton of good code floating around written in C. For many current projects, hardware-oriented code is all written in C. Additionally, many great networking, numerical computing, and cryptography libraries are all written in C. Interfacing with these libraries is as easy as calling the `ccall()` function. The following example callsthe clock function in `libc` with no arguments and returns the result.

```julia
t = ccall((:clock, "libc"), Int32, ())
```

## Running Bash Scripts and System Commands
Another common usecase in which Julia excels is running terminal commands. Much like how C code to complete a task is readily available, chances are good that if you're trying to do something high-level, someone's already done it in Bash. It is important to be able to take advantage of this in a higher level scripting language, as this allows for code re-use. Julia includes an extensive system for calling system commands. This system allows for building commands argument by argument, managing pipelines of commands, and running system commands asynchronously. The following example shows how to run multiple commands in parallel, then pipe both results to a third command for processing. For documentation on this see the official [Julia Docs](https://docs.julialang.org/en/v1/manual/running-external-programs/).

```julia
run(pipeline(`echo world` & `echo hello`, `sort`));
```

