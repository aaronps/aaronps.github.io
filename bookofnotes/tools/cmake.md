# cmake

Config goes on file `CMakeLists.txt`.

If generate for Visual Studio (17) is ok but when try to generate for mingw-w64
It fails (tries to use mingw32-make from codeblocks, but after reconfigure
still failinig). 

## Basic usage like Makefile

_untested example_
```cmake
cmake_minimum_required(VERSION 2.8)
project(cmakesdltest)
add_executable(myExeName src/file1.cpp src/file2.cpp)

target_include_directories(myExeName PRIVATE /some/include/path)
link_directories(/some/lib/dir)
target_link_libraries(myExeName lib1name lib2name)
```

## Show the build commands

Multiple methods

* `make VERBOSE=1`
* `cmake --build 'folder' -- VERBOSE=1`
* `CMakeLists.txt`: `set( CMAKE_VERBOSE_MAKEFILE on )`

## Build in release mode

    cmake -D CMAKE_BUILD_TYPE=Release source_folder

## Build with gprof

    cmake -DCMAKE_CXX_FLAGS=-pg -DCMAKE_EXE_LINKER_FLAGS=-pg -DCMAKE_SHARED_LINKER_FLAGS=-pg <SOURCE_DIR>

Build and run the program normally, after it finishes

    gprof your_executable program > report.txt

## Visual studio

Regarding subsystem (console, windows) take a look at https://cmake.org/Wiki/VSConfigSpecificSettings

If using generated project files and need to **install** to copy `.dll` files,
run `cmake-gui` again and select the output folder as `CMAKE_INSTALL_PREFIX`
regenerate the project files then **build** `INSTALL` target on vs. 

## external library example

SDL2, uncompress the VC file, create `sdl2-config.cmake` on that folder

```cmake
set(SDL2_INCLUDE_DIRS "${CMAKE_CURRENT_LIST_DIR}/include")

# Support both 32 and 64 bit builds
if (${CMAKE_SIZEOF_VOID_P} MATCHES 8)
  set(SDL2_LIBRARIES "${CMAKE_CURRENT_LIST_DIR}/lib/x64/SDL2.lib;${CMAKE_CURRENT_LIST_DIR}/lib/x64/SDL2main.lib")
else ()
  set(SDL2_LIBRARIES "${CMAKE_CURRENT_LIST_DIR}/lib/x86/SDL2.lib;${CMAKE_CURRENT_LIST_DIR}/lib/x86/SDL2main.lib")
endif ()

string(STRIP "${SDL2_LIBRARIES}" SDL2_LIBRARIES)
```

start cmake-gui, select the Source folder (where CMakeLists is), and the "binaries" (where the generated build files will be), then it will do some work and fail.

Note there is one SDL2_DIR, set it to the correct location (the one with the sdl2-config.cmake).

Configure other parameters if you want, the click generate.

### Generating by command line

for 32bit:

    mkdir vs2017
    cd vs2017
    cmake -G "Visual Studio 15 2017" -D SDL2_DIR="c:\path\to\SDL2-X.X.X-VC" ..

for 64bit:

    mkdir vs2017-64
    cd vs2017-64
    cmake -G "Visual Studio 15 2017 Win64" -D SDL2_DIR="c:\path\to\SDL2-X.X.X-VC" ..

### If on the other hand open directly on VisualStudio

_After basic tests, I don't like how VS behaves this way, better avoid_

WARNING: This seems doesn't work on vs2017 anymore, maybe my fault, check another way below.

Edit settings and add something like this to each configuration:

```json
"variables": [
    {
        "name": "SDL2_DIR",
        "value": "path\\to\\SDL2-X.X.X-VC"
    }
]
```

Another way after open the project is to right click on `CMakeLists.txt` and select **Change CMake Settings**,
then on the command args you can add what you need, like: `-DSDL2_DIR=path\\to\\SDL2`.

NOTE: Maybe only need to select menu **CMake** and **Change CMake Settings**.

Also, take into account that these builds go automatically to "${env.USERPROFILE}\\CMakeBuilds\\${workspaceHash}\\build\\${name}" (the default on the settings) which is somewhere on your home.

If don't want some of the targets, select **Project -> Edit Settings -> CppProperties.json** and remove undesired ones, then close and open again folder.

### external library on windows with mingw-w64

Uncompress SDL2 for mingw and add this `sdl2-config.cmake` to its folder:

```cmake
# Support both 32 and 64 bit builds
if (${CMAKE_SIZEOF_VOID_P} MATCHES 8)
  set(SDL2_BASE "${CMAKE_CURRENT_LIST_DIR}/x86_64-w64-mingw32")
else ()
  set(SDL2_BASE "${CMAKE_CURRENT_LIST_DIR}/i686-w64-mingw32")
endif ()

set(SDL2_INCLUDE_DIRS "${SDL2_BASE}/include/SDL2")
set(SDL2_LIBRARIES "-L${SDL2_BASE}/lib -lmingw32 -lSDL2main -lSDL2 -mwindows")

string(STRIP "${SDL2_LIBRARIES}" SDL2_LIBRARIES)
```

Then to use it

```cmake
find_package(SDL2 REQUIRED)
target_include_directories(targetname PRIVATE ${SDL2_INCLUDE_DIRS})
target_link_libraries(targetname ${SDL2_LIBRARIES})
```

When generating the makefiles, it runs better from command line, need to add the SDL2_DIR variable so cmake can find it

1. Make sure the _mingw-w64_ you want is on the `PATH`
2. Create a directory to hold the resulting makefiles and binary, preffer a directory different from the base of your source code.
3. Run command adding the corect `SDL2_DIR`:

    cmake -G "MinGW Makefiles" -D "SDL2_DIR=C:\path\to\SDL2-2.X.X-mingw" path\to\source

4. Run `mingw32-make`

### external library on windows for msvc and mingw

Having prepared the two previous `sdl2-config.cmake` with a common parent folder, add another `sdl2-config.cmake` there, for example:

* SDL2-2.0.7
    * SDL2-2.0.7-VC
    * SDL2-2.0.7-mingw
    * sdl2-config.cmake

The new `sdl2-config.cmake` just forwards to the correct one depending on the compiler used.

```cmake
if ( MINGW )
	include( ${CMAKE_CURRENT_LIST_DIR}/SDL2-2.0.7-mingw/sdl2-config.cmake)
elseif ( MSVC )
	include( ${CMAKE_CURRENT_LIST_DIR}/SDL2-2.0.7-VC/sdl2-config.cmake)
endif ()
```

### find_package on windows

Add key to registry (SDL2 as package example)
`HKEY_LOCAL_MACHINE\SOFTWARE\Kitware\CMake\Packages\SDL2` set default value to
path with the cmake file.

Can be HKEY_LOCAL_MACHINE or HKEY_CURRENT_USER.

### c++11

One way

```cmake
target_compile_features(targetname PRIVATE cxx_std_11)

# _PRIVATE_ | _PUBLIC_ | _INTERFACE_
```

Another way for older versions of cmake

    set_target_properties(target_name PROPERTIES CXX_STANDARD 11)

Another way for all

```cmake
set (CMAKE_CXX_STANDARD 11)
set (CMAKE_CXX_STANDARD_REQUIRED ON)
```

### force 32bit on mingw-w64

Some versions doesn't support both 32 and 64 bit, for the ones that does, you need to create a _platform file_:
https://stackoverflow.com/questions/5805874/the-proper-way-of-forcing-a-32-bit-compile-using-cmake

Other versions you need to have 2 compilers, one for 64 and one for 32. When creating the builds,
add the compiler you want to use to `PATH` (for example 32 bit) and creste the build. Example:

    mkdir build32
    cd build32
    PATH path/to/i686-mingw-w64/bin;%PATH%
    cmake ..

    (in another terminal)
    mkdir build64
    cd build64
    PATH path/to/x86_64-mingw-w64/bin;%PATH%
    cmake ..

Now you can build any version from the base folder

    cmake --build build32
    cmake --build build64

## install target

```cmake
install(TARGET targetname ARCHIVE DESTINATION some/path)
install(DIRECTORY dir_name/ DESTINATION some/path)
```

IMPORTANT: note the '/' at the end of `DIRECTORY dir_name/`, if you don't add it,
the contents will be copied to `some/path/dir_name` which you may or may not want.

On linux, if wants to install somewhere else than default (/usr/local),
regenerate with different prefix:

    cmake -D CMAKE_INSTALL_PREFIX=/ <source_dir>

Then when install add DESTDIR

    make install DESTDIR=some/other/folder

## Netbeans

When creating a project with existing sources with cmake, select custom,
then you can use a different directory for the build. If you don't do like this,
the cmake files will polute you base folder.

## RPATH

**RPATH** will be written by default including all libraries cmake thinks it
needs, this is good for developing. When you **install** it can be replaced:

```cmake
set_target_properties(target_name PROPERTIES INSTALL_RPATH "$ORIGIN/lib")
```

## mingw

If not on path, before running cmake add your desired mingw/bin to path

## Code::Blocks

Generate project

    cmake -G "CodeBlocks - MinGW Makefiles" path/to/source

Then within CodeBlocks there will be several targets.

IMPORTANT: For the special targets, use **build** button and not the **run** button.

edit_cache -> will launch `cmake-gui` to reconfigure
install -> will run `make install`, change destination by `edit_cache`

### Generated Targets

target_name: The normal build procedure
target_name/fast: Build procedure without checking external dependencies
install: The normal install
install/strip: Installs and strip the binaries
install/local: _Need to check_
