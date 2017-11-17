# pywinpty
Python bindings for the [winpty](https://github.com/rprichard/winpty) pseudo terminal library. It allows to create and communicate with Windows processes that print outputs and recieve inputs via console input and output pipes.

## Build status
[![Build status](https://ci.appveyor.com/api/projects/status/cowkuaebgeeq45v1?svg=true)](https://ci.appveyor.com/project/spyder-ide/pywinpty)
[![Coverage Status](https://coveralls.io/repos/github/spyder-ide/pywinpty/badge.svg?branch=HEAD)](https://coveralls.io/github/spyder-ide/pywinpty?branch=HEAD)

## Project info
[![Project License](https://img.shields.io/pypi/l/pywinpty.svg)](./LICENSE.txt)
[![pypi version](https://img.shields.io/pypi/v/pywinpty.svg)](https://pypi.python.org/pypi/pywinpty)

## Dependencies
To compile pywinpty sources, you must have [Cython](https://github.com/cython/cython) and Visual Studio (2013 or later) installed. You must also have Winpty's C header and library files available on your include path.

## Installation
You can install this library by using conda or pip package managers, as it follows:

Using conda (Recommended):
```bash
conda install pywinpty -c spyder-ide
```

Using pip:
```bash
pip install pywinpty
```

Due to Visual Studio version incompatibilities, we currently only support Python 3.5 and 3.6 versions. Both installation packages include the required winpty binaries.

## Building from sources
To build from sources you will need to open either a Visual Studio development prompt or execute the ``vcvarsall.bat`` script from a CMD/Powershell console, then you will need to setup the following environment variables:

```batch
set DISTUTILS_USE_SDK=1
set LIBRARY_INC=<Path to the folder that contains wintpty headers>
set LIBRARY_LIB=<Path to the folder that contains wintpty library files>
```

To test your compilation environment settings, you can build pywinpty Cython sources locally, by executing:

```bash
python setup.py build_ext -i --compiler=msvc
```

If everything works correctly, you can install winpty by using ``pip``:

```bash
pip install -U .
```

## Package usage
Pywinpty offers a single python wrapper around winpty library functions. This implies that using a single object (``winpty.PTY``) it is possible to access to all functionallity, as it follows:

```python
# High level usage using `spawn`
from winpty import PtyProcess

proc = PtyProcess.spawn('python')
proc.write('print("hello, world!")\r\n')
proc.write('exit()\r\n')
while proc.isalive():
	print(proc.readline())

# Low level usage using the raw `PTY` object
from winpty import PTY

# Start a new winpty-agent process of size (cols, rows)
cols, rows = 80, 25
process = PTY(cols, rows)

# Spawn a new console process, e.g., CMD
process.spawn(r'C:\windows\system32\cmd.exe')

# Read console output (Unicode)
process.read()

# Write input to console (Unicode)
process.write('Text')

# Resize console size
new_cols, new_rows = 90, 30
process.set_size(new_cols, new_rows)

# Know if the process is alive
alive = process.isalive()

# Close console pipes
process.close()

# End winpty-agent process
del process
```

## Changelog
Visit our [CHANGELOG](CHANGELOG.md) file to know more about our new features and improvements.

## Contribution guidelines
We follow PEP8 and PEP257 for pure python packages and Cython/VS to compile extensions. Feel free to send a PR or create an issue if you have any problem/question.
