# SDL 2

## Download

| Description                  | File |
| --                           | -- |
| Source code for reference    | SDL2-X.X.X.zip|
| Compiling with Visual Studio | SDL2-devel-X.X.X-VC.zip|
| Compiling with mingw-w64     | SDL2-devel-X.x.x-minwg.tar.gz|
| 64bit binary                 | SDL2-X.X.X-win32-x64.zip|
| 32bit binary                 | SDL2-X.X.X-win32-x86.zip|

## Code::Blocks mingw-w64

Download and uncompress `SDL2-devel-2.x.x-minwg.tar.gz` somewhere.

In Code::Blocks app type: _console application_

### Build Options

Note: replace `x86_64` to `i686` for 32bit builds.

* Search directories -> Compiler: add path to `SDL2-X.X.X/x86_64-w64-mingw32/include/SDL2`.
* Search directories -> Linker: add path to `SDL2-X.X.X/x86_64-w64-mingw32/include/SDL2`.
* Linker Settings -> Link libraries
    * mingw32
    * SDL2main
    * SDL

After building, copy `SDL2.dll` (and the Readme) from `SDL2-X.X.X-win32-x64.zip` (or `x86` for 32bits) to your binary output. 

## Visual Studio 2017

Download and uncompress `SDL2-devel-X.X.X-VC.zip` somewhere.

### Project properties

* (All Platforms) C/C++ -> General -> Additional include directories: add path to `SDL2-X.X.X-VC\include`
* (All Platforms) Linker -> Input -> Additional Dependencies: add `SDL2.lib;SDL2Main.lib` 
* Linker -> General -> Additional library directories: add path to `SDL2-X.X.X-VC\lib\x64` (or `x86` for 32bits)

After bulding copy the same dll files as mingw build.

