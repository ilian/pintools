# Building and running Intel Pintools on Windows

Compiling and running the sample pintools provided by Intel does not seem to function correctly.
Surprisingly, compiling the MyPinTool sample using the provided Visual Studio project file succeeds,
but instrumenting an application causes the application to crash.
Building the pintools in the following way resolves this issue. We will use the provided MyPinTool as an example.

## Requirements
Install the following components:
* Microsoft Windows (tested with Windows 7 x64)
* [Microsoft Visual Studio](https://visualstudio.microsoft.com/) (tested with Visual Studio 2015)
* [Cygwin](https://cygwin.com/) with GNU make
* [Intel PIN](https://software.intel.com/en-us/articles/pin-a-binary-instrumentation-tool-downloads) (tested with Pin 3.11)

## Building MyPinTool
Determine the architecture of the compiled target binary that you will instrument and open the Native Tools Command Prompt
corresponding to that architecture, which should be present in the start menu after installing Visual Studio.
A new shell will be spawned with the correct environment. As we will be using Makefiles to build the pintool,
we need to add the make tool to the current path by executing `PATH=%PATH%;C:\cygwin64\bin` in the spawned shell, or
permanently add `C:\cygwin64\bin` to your PATH environment variable to avoid having to add it every time.

* Navigate to the source files of the pintool, namely in `source\tools\MyPinTool`
* Build the pintool for the desired architecture using make
  * `make obj-ia32/MyPinTool.dll TARGET=ia32` for 32-bit targets
  * `make obj-intel64/MyPinTool.dll` for 64-bit targets

## Instrumenting an executable
Navigate with a shell to the root of your Pin installation if you compiled a 32-bit pintool.
For 64-bit pintools, navigate to `intel64\bin\` relative to the root of your Pin installation.
Execute pin.exe with flag `-t` to specify the location of the compiled pintool and `-o` to specify the output file.

Example instrumenting `calc.exe`:
```
C:\pin\intel64\bin>pin.exe -t C:\pin\source\tools\MyPinTool\obj-intel64\MyPinTool.dll -o out.log -- calc
```
