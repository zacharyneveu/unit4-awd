# Migrating Legacy Python 2 Scripts

Python 2 will not be maintained past 2020. Many scripts around the company rely on Python 2. Most of these projects should consider migrating to a supported version of Python or another language.

## Why Migrate?
Along with The lack of official support for the Python 2 language, many prominent Python packages will no longer work with this version of the software. This is likely to cause problems for many projects. The list of packages that are dropping support is [extensive](https://python3statement.org/), but some highlights include: TensorFlow, Pandas, SciPy, NumPy, MatPlotLib, pytest, and Jupyter. When adapting old scripts for use on a new project, new features of these libraries will inevitably become useful, but will not work with Python 2.

## Are there Exceptions?
Legacy embedded devices may only support Python 2. In these cases, it will be necessary to freeze all packages to versions that are compatible with Python 2.

## What Replaces Python 2?
The obvious replacement to Python 2 is Python 3. Python 3 has been in release for over 10 years, and provides additional functionality over Python 2. Python 3 has good package support, good community support, and is widely used. One potentially misplaced concern with Python 3 has been that its performance on particular tasks has historically been far slower than Python 2. In particular, Python 3.0 had poor IO performance (see StackExchange post by core developer [here](https://softwareengineering.stackexchange.com/questions/63859/why-do-people-hesitate-to-use-python-3). As of the latest stable Python 3 release (3.7), performance concerns have been mitigated considerably. For a breakdown of this by version and task, see this [hacker noon post](https://hackernoon.com/which-is-the-fastest-version-of-python-2ae7c61a6b2b). If the performance of Python 3 is a concern, it is likely that Python 2 is also too slow for your needs. One option is to try out [PyPy](https://pypy.org/), a JIT compiled version of Python which can be much faster than standard Python in several cases. Another option that is increasingly viable is to migrate the project to Julia. The [Julia Language](https://julialang.org/) offers performance approaching C, while retaining the flexibility and generality of Python.

## Julia for Performance-Sensitive Tasks
The Julia language is, as the creators say, created for "greedy" programmers. The goal is to approach the speed of C, while allowing for the ease of use of Python or Matlab. In addition to this performance boost, Julia has several other advantages, such as the ability to directly call functions in C code, system scripts, and Python scripts. This functionality makes Julia a great high-level scripting language, in addition to a fast low-level language. The ability to handle both of these uses in a single language can replace gluing together the heaps of C programs, shell scripts and Python that hold most current projects together. Below are some examples of how Julia could simplify the stack on your next project.

## Julia Examples
### Calling C Functions
While Julia is efficient and can be very fast, there is already a ton of good code floating around written in C. For many current projects, hardware-oriented code is all written in C. Additionally, many great networking, numerical computing, and cryptography libraries are all written in C. Interfacing with these libraries is as easy as calling the `ccall()` function. The following example calls the clock function in `libc` with no arguments and returns the result.

```julia
t = ccall((:clock, "libc"), Int32, ())
```

### Running Bash Scripts and System Commands
Julia also excels at running terminal commands. Most current projects make extensive use of Bash scripts for high-level tasks. It is important to be able to take advantage of this in a higher level scripting language, as this allows for code re-use. Julia includes an extensive system for calling system commands and bash scripts. This system allows for building commands argument by argument, managing pipelines of commands, and running system commands asynchronously. The following example shows how to run multiple commands in parallel, then pipe both results to a third command for processing. For documentation on this see the official [Julia Docs](https://docs.julialang.org/en/v1/manual/running-external-programs/).

```julia
run(pipeline(`echo world` & `echo hello`, `sort`));
```


### Calling Python Functions
Python is the 3rd most used programming language in the world, while Julia is still relatively small. The Julia developers have made sure that Julia plays well with Python. Via the [PyCall](https://github.com/JuliaPy/PyCall.jl) package, it is possible to import Python libraries and use them as if they were native Julia libraries. This can be incredibly useful, particularly if someone has already taken the time to write a set of utility functions in hardware for interacting with a device or program. The following example is trivial, but shows the straightforward process of importing a Python module and calling functions from Julia.
```julia
using PyCall
math = pyimport("math")
math.sin(math.pi / 4)
```

### Is Julia Right for Your Project?
For many projects, Python 3 may be your best choice. Porting is straightforward, and if your team already knows Python, this may make the most sense. For new projects, however, Julia offers an impressive set of features that were not available 10 years ago when Python 3 was introduced. In either case, the end of support for Python 2 will be a critical moment in the lives of many projects. Before migrating code, make sure that it is built on the right foundation.
