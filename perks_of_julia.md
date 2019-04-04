# Migrating Legacy Python 2 Scripts

Python 2 will not be maintained past 2020. Many scripts around the company rely on Python 2, and many should consider migrating to a supported version or another language.

# Why Should Scripts be Migrated?
The lack of support for Python 2 itself does not cause monumental problems. Many prominent Python packages are also dropping support for Python 2, however, and this is likely to cause problems. The list of packages that are dropping support is [huge](https://python3statement.org/), but some highlights include: TensorFlow, Pandas, SciPy, NumPy, MatPlotLib, pytest, and Jupyter. When adapting old scripts for use on a new project, new features of these libraries will inevitably become useful.

# Are there any exceptions?
Legacy embedded devices may only support Python 2. In these cases, it will be necessary to freeze all packages to versions that are compatible with Python 2. Additionally, if it is unlikely that a script will be used in future projects, it is probably not worth the effort to convert.

# What Replaces Python 2?
The obvious replacement to Python 2 is Python 3. Python 3 has been in release for over 10 years, and provides additional functionality over Python 2. Python 3 has good package support, good community support, and is widely used. One potentially misplaced concern with Python 3 has been that its performance on particular tasks has historically been far slower than Python 2. In particular, Python 3.0 had poor IO performance (see StackExchange post by core developer [here](https://softwareengineering.stackexchange.com/questions/63859/why-do-people-hesitate-to-use-python-3). As of the latest stable Python 3 release (3.7), these concerns are largely not a problem. For a specific task breakdown of this, see this [hacker noon post](https://hackernoon.com/which-is-the-fastest-version-of-python-2ae7c61a6b2b). If the performance of Python 3 is a concern, then it is likely that Python 2 may also be too slow for your needs. One option is to try out [PyPy](https://pypy.org/), a JIT compiled version of Python which can be much faster than the traditional distribution. Another option that is increasingly viable is to migrate the project to Julia. The [Julia Language](https://julialang.org/) offers performance approaching C, while retaining the flexibility and generality of Python.

# Julia for Performance-Sensitive Tasks

This is back to regular text.

