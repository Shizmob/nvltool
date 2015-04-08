nvltool
=======

This is a simple tool that allows you to create, modify and extract obfuscated [NVList](http://nvlist.weeaboo.nl) archive (.nvl) files.

Usage
-----
```
usage: nvltool (-l | -x | -c | -a | -d) [-k KEY] [-o OUT] [-v] [-h] [-V]
               ARCHIVE [FILE [FILE ...]]

A tool for working with NVList archive files.

positional arguments:
  ARCHIVE               The NVList archive file to operate on.
  FILE                  The files to operate on.

optional arguments:
  -k KEY, --key KEY     The obfuscation key to use, in hexadecimal.
  -o OUT, --out OUT     The archive file to output to when appending or
                        deleting, or output directory when extracting.
  -v, --verbose         Show more information about what is going on.

actions:
  -l, --list            List files in archive.
  -x, --extract         Extract files from archive.
  -c, --create          Create archive from files.
  -a, --append          Add files to archive.
  -d, --delete          Delete files from archive.

misc:
  -h, --help            Print this help and exit.
  -V, --version         Show version information and exit.

The FILE argument can optionally be in ARCHIVE=REAL format, mapping a file in
the archive file system to a file on your real file system. For example:
nvltool -x test.nvl img.png=/home/foo/sexy.png
```

Examples
--------
    nvltool -x foo.nvl
Extract every file from `foo.nvl` into the current directory.

    nvltool -v -o output -x foo.nvl main.lua ui.png
Extract the files `main.lua` and `ui.png` from `foo.nvl` into the directory `output`, listing its progress to the terminal.

    nvltool -c bar.nvl test.jpg main.lua sprites
Create the archive `bar.nvl`, containing the files `test.jpg`, `main.lua` and the directory `sprites`.

    nvltool -k 12BD85DEADBEEF -c bar.nvl movies=C:\projects\vn\movies
Create the archive `bar.nvl` with obfuscation key `0x12BD85DEADBEEF`, taking files from `C:\projects\vn\movies` and putting them into the directory `movies`.

    nvltool -l baz.nvl
List all the files in the archive `baz.nvl`.

    nvltool -o barv2.nvl -d bar.nvl foo.jpg
Remove the file `foo.jpg` from the archive `bar.nvl`, storing the result archive in `barv2.nvl`.

API
---
`nvltool` can also be included in your own projects to provide the `NVLArchive` class.

A small API overview:

```
NVLArchive(file=None, key=None)
  Create an archive instance. If file is not None, it will be passed to open(). If key is None, it will be generated from the default NVList key.

NVLArchive.open(file)
  Open archive. File can either be a filename or a file-like object. Will raise a zipfile.BadZipFile exception if the underlying zip archive is not valid.

NVLArchive.close()
  Close the archive. Will raise a ValueError if no archive is currently open. Any modifications will NOT be saved unless you call NVLArchive.save() beforehand.

NVLArchive.is_open()
  Return whether or not an archive is currently open and associated with this instance.


NVLArchive.list()
  Return a frozenset containing all the files (not folders) currently present in the archive.

NVLArchive.has(path)
  Return whether or not the given path exists as a file in the archive.

NVLArchive.read(path)
  Read the given file from the archive and return its contents as bytes. Will raise a FileNotFoundError if the file is not present in the archive.

NVLArchive.write(path, contents)
  Write contents as bytes to the given filename in the archive. Will overwrite any existing files with the same filename.

NVLArchive.remove(path)
  Remove the given file from the archive. Will raise a FileNotFoundError if the file is not present in the archive.

NVLArchive.save(file=None)
  Save any made modifications. If file is given, the new archive will be saved to that file instead.
  Will raise a ValueError if no file was given and no archive was opened through open().
  If no archive was opened before, after saving the archive will open the given file.
```

License
-------
nvltool is licensed under the WTFPL. See the LICENSE file for moer details.

Disclaimer
----------
This tool is ONLY intended for use with files on which the authors allowed modification of and/or extraction from and the unpermitted use on files where such consent was not given is highly discouraged, and most likely a license violation as well.
Support requests for help with dealing with such files will not be answered.

Credits
-------
Credits for the creation of the NVList archive format go to [anonl](http://weeaboo.nl).
