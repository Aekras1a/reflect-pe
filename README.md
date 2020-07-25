# Reflect-pe
![Build](https://github.com/ayoul3/reflect-pe/workflows/Go/badge.svg)  

Reflectively load and execute an x64 Portable Executable in-memory. It does not bluntly allocate RWX memory all over the place. Each section gets its required set of permissions.   It also allows patching bytes on the fly to evade memory inspection (Windows Defender).  
Finally, it produces a self-contained executable so no ugly command lines to be inspected by Defender.  

## Usage  
1. [Prepare a Go environment](https://golang.org/dl/) to build the reflect-pe on Windows. You cannot cross compile it on a Linux/Unix machine because it uses Windows specific code.  

2. Prepare your `config.yml` file (see below)

3. Build the executable
```
git clone https://github.com/ayoul3/reflect-pe
cd reflect-pe
go build -v .
```
Run it and enjoy

## Config
```
# BinaryPath can either be an HTTP url, relative path or absolute path.

BinaryPath: 'whatever.exe'

# ReflectArgs are the arguments passed to your binary when executed in memory. First argument is the program's name. This parameter can be empty

ReflectArgs: 'heyho'

# 3 options to load an executable in memory:
# Wait: will call CreateThread, then wait 15 to 30 seconds before resuming the threat. This proved very effective against Windows Defender.

# <empty>: We do not wait between thread creation and execution

#  Function: We overwrite the address of a Go method with the executable's entry point


ReflectMethod:  # wait, function or empty

# 0: no logs, 1: Info logs, 2: Debug
LogLevel: 2

# Keywords to be replaced by a random shuffle. Both utf8 and unicode versions are replaced.

Keywords:
  - mimikatz
  - delpy
  - benjamin
  - delpy
```
Ps: config_mimi.yml contains the basic keywords to target to run a mimikatz without triggering Windows Defender

## Limitations
Reflect-pe only works for x64 dynamic executables on 64-bit intel machines.  

It's not stable when it comes to static binary for the good reason that hardcoded absolute addresses are difficult to find and translate to the new relocated address. So it cannot load a go-binary for instance.  

I still did not try loading a DLL. There may be some small work to do to make it fully work.  


## Credit
* https://github.com/stephenfewer/ReflectiveDLLInjection  
* https://github.com/PowerShellMafia/PowerSploit/blob/master/CodeExecution/Invoke-ReflectivePEInjection.ps1