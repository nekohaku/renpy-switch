# Ren'Py for Nintendo Switch
This is a port of [Ren'Py](https://www.renpy.org/), a visual novel game engine, to the Nintendo Switch for **homebrew** purposes.  
If you are looking for a **commercial port** for publishing to the Nintendo eShop, contact [Ratalaika Games](http://www.ratalaikagames.com/).  
The repository is located at the following URL: https://github.com/uyjulian/renpy-switch  
Ren'Py is a visual novel engine that is written using [Python](https://www.python.org/).  
Discussion of this project is on the [Lemma Soft Forums](https://lemmasoft.renai.us/forums/viewtopic.php?f=32&t=55503).  

## Building
Please view the `external_library_build/Building.md` file for instructions on building this project, including the additional third party dependencies.  
Once the steps in the aforementioned file are completed, run `make` to build the project.  

## File Formats
It is highly recommended that you use the following file formats:  

* WebP for image assets
* Opus/WebM for audio assets
* VP8/Opus/Matroska for video assets
* RPAv2 for game archives

If these formats are not used, there is a possibility of the program working incorrectly or performance being impacted.  
Free tools such as [FFmpeg](http://ffmpeg.org/), [cwebp](https://developers.google.com/speed/webp/docs/precompiled), and [ImageMagick](https://imagemagick.org/) are available to convert file formats.  
[rpatool](https://github.com/Shizmob/rpatool) can be used to create and merge RPA files, and also convert to RPAv2 format.  
The file format can be changed without changing the file extension, so no script (`rpy` file) changes are needed.  
The file `example.png` can be in the WebP file format without changing the filename to `example.webp`.  

## RomFS Integration (from source)
To integrate the game into one single `nro` file, place game files in a folder named `Contents` in the directory `romfs` (see section File system layout), and build as described in the "Building" section.  
NOTE: If you do not compile the `py` and `rpy` files to `pyo` and `rpyc` respectively by running the game at least once (using either this port or upstream Ren'Py) on a read-write file system, the loading time required until the title screen is seen will be increased.  

## RomFS Integration (from SDK archive)
To integrate the game into one single `nro` file, follow these steps:
1. Place game files in a folder named `Contents` in the folder named `romfs` (see section File system layout).
2. Install package `switch-tools` from devkitPro pacman (follow the installation instructions [here](https://devkitpro.org/wiki/Getting_Started) if you have not already)
3. Add the tools to your PATH: `export PATH=${DEVKITPRO}/tools/bin:$PATH`
4. Generate `control.nacp`: `nacptool --create TITLE AUTHOR VERSION control.nacp`
5. Package everything into an `nro` file: `elf2nro renpy-switch.elf OUTPUT.NRO --romfsdir=romfs --nacp=control.nacp --icon=LOGO.JPG`

## File system layout
The following files or folders are required to be in the same directory as the `.nro` or in RomFS:  
* `lib.zip` - contains the Python stdlib, Ren'Py modules, pygame_sdl2 modules, and libnx binding modules.
* `renpy` - contains the `common` directory used by Ren'Py.
* `renpy.py` - startup script for Ren'Py.
* `game` - contains the game files. This is where you place the game.

## libnx bindings
Ren'Py for Nintendo Switch supports mostly complete [libnx](https://github.com/switchbrew/libnx) bindings.  
The bindings are generated by [SWIG](http://www.swig.org/).  

To use the bindings, you need to import the library `libnx`.  
```py
import libnx
```
To initialize a structure, you can use the following syntax:  
```py
clkrstSession = libnx.ClkrstSession()
```
To access members of a structure, you can use the following syntax:  
```py
service = clkrstSession.s
```
To access a enumeration, you can use the following syntax:  
```py
libnx.PcvModuleId_CpuBus
```
To call a function, you can use the following syntax:  
```py
libnx.clkrstOpenSession(clkrstSession, libnx.PcvModuleId_CpuBus, 3)
```

Example use cases of this feature:  
* Mounting game RomFS  
* Creating save data on the system memory  
* Opening a video in the web browser applet (with hardware acceleration)  
* Opening the software keyboard applet  

## Available Python native modules
The following native Python modules are available in Ren'Py for Nintendo Switch:  
* `_bisect`
* `_codecs`
* `_codecs_cn`
* `_codecs_hk`
* `_codecs_iso2022`
* `_codecs_jp`
* `_codecs_kr`
* `_codecs_tw`
* `_collections`
* `_csv`
* `_ctypes_test`
* `_elementtree`
* `_functools`
* `_heapq`
* `_hotshot`
* `_io`
* `_json`
* `_locale`
* `_lsprof`
* `_md5`
* `_multibytecodec`
* `_random`
* `_sha`
* `_sha256`
* `_sha512`
* `_socket`
* `_sre`
* `_struct`
* `_symtable`
* `_testcapi`
* `_weakref`
* `array`
* `audioop`
* `binascii`
* `bz2`
* `cPickle`
* `cStringIO`
* `cmath`
* `datetime`
* `errno`
* `fcntl`
* `future_builtins`
* `imageop`
* `itertools`
* `math`
* `operator`
* `parser`
* `posix`
* `pyexpat`
* `strop`
* `time`
* `timing`
* `unicodedata`
* `xx`
* `xxsubtype`
* `zipimport`
* `zlib`

## License
This project is licensed under the MIT license. Please read the `LICENSE` file for more information.  
