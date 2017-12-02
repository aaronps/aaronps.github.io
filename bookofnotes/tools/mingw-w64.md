# mingw-w64 on windows

I download the files directly from https://sourceforge.net/projects/mingw-w64/files/
then uncompress then and handle paths by myself, so I can have multiple versions of
the compilers.

I select:

* `posix` for using c++11 threads
* on x86_64: `seh` because it may be faster
* on i686: `sjlj` because may be more compatible

After getting the files, uncompress on any folder, add to `PATH` when needed.

# Runtime dll's

Normal builds will require some dll to be copied to work, these dll's can be found
on the `mingw-w64-path/bin`. One way to find which of them is needed is to try to
run the program from explorer and error messages will popup telling you about the
missing files (or maybe just run `depends`).

## x86_64

* libgcc_s_seh-1.dll
* libstdc++-6.dll
* libwinpthread-1.dll

## i686

* libgcc_s_sjlj-1.dll
* libstdc++-6.dll
* libwinpthread-1.dll

### Static linking runtime dll's

* libgcc_s*-1.dll

    add link option `-static-libgcc`

* libstdc++-6.dll

    add link option `-static-libstdc++`

* libwinpthread-1.dll

    If you are using pthreads somewhere, add link option:

        -l:libwinpthread.a

    **BUT** if not it will fail and you need a longer line

        -Wl,--whole-archive -l:libwinpthread.a -Wl,--no-whole-archive
    
    **NOTE** be aware that the `--whole-archive` one seems to be setting the final `exe` properties
    from the `winpthread-1.dll`

## Notes

When using the **posix** version, if the `bin` folder is _not on path_, the compiler may fail because it won't find `winpthread-1.dll`.
