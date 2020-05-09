# Unix Terminal Guide

## Unix Paths
The unix filesystem is a tree of directory which organize folders and files. Each folder/file in the filesystem can be referenced by a name called a **Path**. A path consists of a series of folder and file names separated by a "/" character. An example of this would be the following:

`~/Documents/images/image.png`

This path would reference an image in the images folder inside of Documents inside of the home directory.

There are two basic types of paths, **Absolute Paths**, and **Relative Paths**. An absolute path always starts at the **root** directory, the highest folder in the system. This will find the exact file specified on the system. A relative path on the other hand would find a file or directory relative to where it is referenced from. The example above is reference the image relative to the home directory.

The following starting symbols correspond to absolute and relative paths:
* `/`: The symbol to start with for an absolute path. Eg. `/home/username/Documents`
* `.`: Represents the current folder for a relative path. Really only useful when running an executable in the same folder. Eg. `./a.out`. When referencing directories, it is never needed, and when using files in the current directory it isn't needed when passing arguments to a command. Eg. `python3 script.py`. Adding a "." won't break anything though. For example, if the user is in the *Documents* directory, the the can reference *images* simply by `images` or the image inside as `images/image.png`
* `..`: Represents the parent directory (the directory one above) for a relative path. Eg. `../images/image.png`. These can also be chained: `../../../images/image.png`
* `~`: Represent the home directory for a relative path. Eg. `~/Documents/images/image.png`. This is also where a terminal will probably startup on a unix environment, although that is not always the case. On linux, the defualt is location of this is `/home/[username]`. On Mac it's at `/Users/[username]`.

## Basic Commands
A general shell command will look something similar to this:

*`command [options] [arguments]`*

For example:

`ls -R -a`

In addition, flags without arguments can often be combined to form single option:

`ls -aR`

This format may vary based on command, but most will look somewhat similar.

A useful command to know is the `man` command, which stands for "Manual". If you want to look up what a command does or what arguments can be passed in, then running `man command` will show you. Pressing "q" while in the manual will quit out of it. Eg. `man ls`

## Printing
### echo
The `echo` command is used as a basic print statement. It can be used with a string or just a list of arguments:

`echo "Hello Shell"`

`echo Hello Shell`

### printf
`printf` is also a printing command, but can format the output based on a pattern. The inputs are as follows:

`printf [pattern] [args]`

Eg.

`printf "%s\n" "Hello" "World"` will print:
```
Hello
World
```

The pattern prints for each argument given. Some patterns that it can match are as follows:
* "%s": Matches with strings as then are given
* "%b": Matches with strings and allows for escape sequences
* "%d": Matches with integers
* "%f": Matches with floats
* "%[padding]x": Matches with integers and outputs in hexadecimal. The padding is the length to add '0's to.

## Navigation
This section covers some commands for navigating the file system
### ls
`ls` is a command that lists all of the files and directories at a specified location. By default, it will search at the current directory where it is run.

Some options that `ls` can be run with are:
* `-a`: lists most things, if not everything in the directory. By default, hidden folders starting with "." (Eg. `.ssh`) are not shown from the ls command. This will allow for them to be printed out.
* `-d`: prints all directories as if they were files with a "." at the front.
* `-R`: Recursive search. Looks through and prints out all sub-directories as well.
* `-l`: Prints long format with more information.
* `-h`: To be used with `-l`. Prints filesizes in largest denominator (bytes, kb, mb, etc.).

An additional, similar sommand is present: `l` which simply runs `ls -l`.

### cd
`cd` stands for "Change Directory" and switches the current directory to a different one if the user has permission to do so. The path the is specified must be a directory, or else the command will fail.

`cd ~/Documents/images`

`cd ../Documents/images`

Running `cd` with no arguments will default to `cd ~` which changes the current directory to home.

### pwd
`pwd` stands for "Print Working Directory" and simply outputs whatever the current directory the user is in. 

### du
`du` stands for "Disk Usage" and outputs the amount of space a file or directory takes up. Simply running the command will show the usage of the files in the directory. A specific file can also be passed in to read the size of that file.

`du`

`du myfile.txt`

### find
`find` will print out all occurences of a pattern in files within a specific path. The general command looks like this:

`find [path] -name "[pattern]"`

Example:

`find ~/Documents -name "image.png"`

## Files and Directories
### Making files and directories
Making a file in the shell can be done through the `touch` command. It can simply be run by:

`touch [1 or more filenames]`

For example:

`touch file.txt`

Another way to make a file is by running `>filename`, but this will be covered in streaming data.

To make a directory, use the `mkdir` command. Basically the same as `touch`.

### Copying and Moving Files/Directories
The `mv` command will move a file or directory to a new file or directory. This is also used for renaming a file or directory. This command takes two arguments, the path of the original file/directory, and the path of the target file/directory. The original file will be deleted.

`mv file1.txt file2.txt`

`mv dir1 dir2`

`mv file1.txt file2.txt dir` moves all of the files into the last argument, in this case dir.

Some things to keep in mind: Moving a file into a directory will move it inside. Moving a directory into an existing directory will move it inside. Moving a directory into a non-existant directory will rename the directory.

By default, a file will overwrite a different file with the same path if it exists. To modify this behaviour, there are a few options that can be added:
* `-f`: Will force overwrite even if system complains if the user privileges allow it
* `-i`: Will prompt and ask the user when a file is to be overwritten
* `-n`: Will not overwrite files

The `cp` command is used for copying files and directories. It works the same way as `mv` but does not delete the originals. There are two additional options that can be used:
* `-R`: Recursive copy
* `-p`: Copies all of the attributes of the original file like time last modified and permissions.

### Deleting Files/Directories
`rm` can mainly be used to delete files, although it has the capability of deleting directories as well. A command specifically made for deleting directories is `rmdir`, but the directory must be empty.

`rmdir mydir`

`rm file.txt`

`rm file1 file2 file3` removes all of the files.

Some options can be used with `rm`:
* `-R`: A recursive deletion, will delete in subdirectories as well.
* `-r`: Same as `-R`.
* `-i`: Gives prompt before deleting.
* `-d`: Tries to delete directories as well.
* `-f`: Forces a deletion if user has privileges.

Note: `-d` is not required when `-r` is present.

### Printing out files
To see the contents of a file, there are two commands that can be used: `cat` and `less`.

`cat` will simply print out the contents of a file to the standard output. `less` will open up the file and allow you to scroll through it with the arrow keys. The `more` command is also often an option for printing. It will do so line by line.

`cat test.txt`

`less test.txt`

`more test.txt`

### Running an Executable
To run an executable file, simple reference it by path. This can be done by a relative or absolute path. As mentioned in *Unix Paths*, if the file is in the same directory as the user, this can be done by `./filename` or if in a different directory by whatever path leads to it. This is also how all commands are run; The commands are stored in a binary folder, often `/bin`, and the operating system looks in that folder when searching for valid commands. Installed commands, like `python` will probably be installed in `/usr/bin`.

If an executable takes in arguments, then you can simply pass them in after the file name.

`./a.out arg1 arg2...`

### File Permissions
When doing `ls -l`, there may be a section that looks like this:

`-rwxr--r-x`

This represents the modes/permissions that are available to certain users for a certain file. The modes come in groups of three, the first being the permissions for the owner of the file, the second for the group that owns it, and the last being everyone else. There are three different symbols, "r" representing read, "w" representing write, and "x" for execute. 

To change the permissions, you can use the `chmod` command, meaning change mode. It can be run in two ways, a specific permission gets added or removed, or by a mode number. The specific permission is easier to understand, simply adding "+/-" and the mode to add or remove as options.

`chmod +x myBinary`

`chmod -wx myfile.txt`

The second way is through a number. It is a three digit Octal number which in binary represents the pattern of modes. for example, 753 represents `rwxr-x-wx`. This is because 7 is rwx, 5 is r-x and 3 is -wx. Some common numbers are 755 and 644.

### Compressing files
## Basic Zipping
`zip` takes a filename output and a list of files to compress. 

`zip output file1, file2...`

`zip myzip.zip file.txt file2.txt`

To uncompress a file, simply run `unzip` on a zip archive to uncompress it. Note that this command can only take one file.

`unzip myzip.zip`

### gzip and tar
Another popular compression command is `gzip` with the extension ".gz". It only works on files and will produce the same amount of files as it is given with the extension ".gz".

`gzip file.txt file2.txt` will compress into "file.txt.gz" and "file2.txt.gz".

To uncompress specific files, use the command `gunzip` which takes in ".gz" files and inflates them to their normal size.

`gunzip file.txt.gz`

Note: `gzip` is not an archiving command, it is only a compression command.

`tar` on the other hand is for archiving folders or a list of files. It is very similar to the `zip` command, but does can create, extract, and list the contents of a ".tar" file in one command, depending on the options.

`tar -cf mytar.tar file.txt file2.txt` makes a new archive.

`tar -tf mytar.tar` prints the contents.

`tar -xf mytar.tar mydir` extracts into a directory. If no directory is specified it will output to the current directory.

Note: `tar` with the options above only archives and doesn't compress. To also compress, as `-z` flag must be added. The resulting file will have an extenion of ".tar.gz" by convention.

`tar -czf mytar.tar.gz file.txt file2.txt`

`tar -xzf mytar.tar.gz mydir`


## Streaming Data
### Streaming input
Streaming an input of data can be done using the `<` symbol. This will link the contents of a file into the standard input of a command. This can be used for the built-in shell commands or even your own programs.

Examples:

`touch < filestomake.txt`

`python3 myscript.py < input.txt`

### Streaming output
To output the results of one command into a file, the `>` symbol can be used. This can be done with any command. This will create or overwrite the file. To append instead of overwrite the `>>` symbol is used.

Examples:

`cat myfile.txt > results.txt`

`./myprogram >> output.txt`

A special place on the filesystem is `/dev/null`. It is a file that can be used for "discarding" outputs. If you redirect an output here, then it will simply not show up.

`ls -a > /dev/null`

An extra added argument that can be added is `2>&1` at the end. `ls -a > /dev/null 2>&1`. What this will do is send all error messages there too. When redirecting files, there are 3 specials numbers: 0 for stdin, 1 for stdout and 2 for stderr. What `2>&1` does is send everything in stderr to stdout, and is therefore send to `/dev/null`.

### Piping
File redirection is good for a way to output logs or get constant inputs in. To get a dynamic input/output stream, a pipe, or `|`, has to be used. This will allow a second command to get input as soon as it is available. It will link the standard output of one of the programs to the standard input of another.

`cat test.txt | ./myprogram`

## Text processing
### wc
`wc`, or Word Count, is used to count characters, lines and words of a file. A specific combination of this can be enabled with the `-l` for line, `-c` for character, and `-w` for word options, but by default, all of them are on.

`wc myfile.txt`

`wc -wl myfile.txt`

### grep
`grep` is used for searching for a word(s) in an input. This input is often piped in. It will output the word along with the line that it is found on.

`cat test.txt | grep word`


## Wildcards
Sometimes we need for commands to take in arguments using a pattern rather than a specific file. This is where wildcards come into play. They can be substituted as part of an argument name to use all files that match that pattern.

### *
`*` is a wildcard that matches to everything in a pattern. Some examples:

`rm *` will remove all files in the current directory.

`gcc *.c -o bin` will compile all C source files in the current directory.

`find ~ -name "*.py"` will find all files extending with ".py"

### ?
`?` is a wildcard that matches to any one character.

`rm main.?` will remove all files including main and a character, Eg. main.c, main.h, main.o, main.a.

### []

`[]` will match any comma separated character or ranges specified in the brackets.

`rm m[a,e-g,u]n.c` will remove man.c, men.c, mfn.c, mgn.c, and mun.c

In addition a "!" can be the first character for a negative match. It will match everything except for what is inside of the brackets.

`rm m[!a,e]n.c`.

### {}

`{}` will match all patterns that are inside of the brackets.

`rm {*.cpp, *.hpp}`
