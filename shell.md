# Shell

## Open questions
- What is symlinking?
- what are environment variables?
  -   https://www.digitalocean.com/community/tutorials/how-to-read-and-set-environmental-and-shell-variables-on-linux#:~:text=Environmental%20variables%20are%20used%20to,like%20the%20current%20working%20directory.

## Contents
- PATH
- Executables
  - Turn script into an executable

## PATH
- The PATH is an environment variable which contains a list of directories containing executables.
- When you run a shell command, the shell searches for an executable matching that command's name from the list of directories in PATH.
  - So if you type `ls`, then shell searches through the directories in PATH for an executable called `ls`. When it finds one, it executes it. 
    - This is the same as if you had typed the full path to the `ls` executable: `/bin/ls`.
- The PATH variable is therefore a convenience: rather than having to remember the exact location of the executable you want to run, you can simply type its name and the Shell will handle that for you, by searching in PATH for the various places that an executable might be stored.

- [What exactly is your Shell PATH?](https://medium.com/@jalendport/what-exactly-is-your-shell-path-2f076f02deb4)


## Executables
- An executable is a file which runs a program when opened.
  - If you can run a file by typing its full path into the Shell, then it's an executable.
- There are 2 kinds of executable
  1. Compiled programs (ie, binaries)
  2. Scripts
- Compiled programs are produced by 

- A binary file is any file that isn't plain text.
  - Examples: `.mp3`, `.jpg`, etc.
- So:
  - not all binaries are executables (because you can't run a JPG).
  - not all executables are binaries (because a plaintext `.rb` file can be an executable when given the correct permissions).
  - BUT: `binary` is often used as a short form of `binary executable file`. So when speaking loosely, the distinction is blurred.

### Directories where CLI executables are normally stored
- `/bin` → user utilities — contains some common executables used to navigate directories and manage files via the command line (cd, ls, rm, etc)
- `/sbin` → system programs and administration utilities — contains executables that handle things like booting, restoring, recovering, and/or repairing the system (launchd, reboot, mount, etc)
- `/usr/bin` → common utilities, programming tools, and applications — contains executables that aren’t vital to the system’s functionality but are still included in the operating system (ssh, php, vim, etc)
- `/usr/sbin` → system daemons & system utilities — contains executables that are generally run as background processes ( cron, php-fpm, coreaudiod, etc)
- `/usr/local/bin`, `/usr/local/sbin` → user-installed executables, libraries, etc. — contains executables that unlike the above folders aren’t included by default with the operating system (custom shell scripts, and programs installed via Homebrew, utilities installed with and needed by certain GUI programs, etc.)

### Turn a script file into an executable
- By default, a ruby script file must be explicitly run with `ruby`.
  - For example, you need to type `ruby my_example_script.rb` in the terminal.
- Turning a conventional `.rb` file into an executable requires 2 steps:
  1. Add a shebang line to the top of the file `#!/usr/bin/ruby` (see below for explanation).
  2. Change file permissions to make the file executable: `chmod -x my_example_script.rb`.
- Now the file can be executed directly from the shell, by typing `./my_example_script.rb`
  - You can even remove the `.rb` suffix.
  
- *Shebang Line*
  - For example: `#!/bin/bash` or `#!/usr/bin/python3`
  - Its purpose is to tell the Shell which interpreter should be used to interpret the script.
  - It consists of a `#!` followed by the path to an interpreter such as bash, python, ruby or perl.

## File Permissions
- What the hell is going on in a permissions line like `drwxr-xr-x`? 
- `-rwxr-xr-x` is an example.
  - Overview: this tells us whether the entry is a file or directory, and the 3 possible permissions (read, write, execute), for the 3 possible user types: owner, group and other.
  - The possible permission characters are: `-`, `r`, `w` and `x`.
    - `-`: no permission
    - `r`: read permission
    - `w`: write permission
    - `x`: execute permssion
  - The first character isn't a permission, it's whether the entry is a file or directory.
    - can be either `-` (a file) or `d` (a directory).

  - Then follows 9 characters. These should be read as the permissions for 3 seperate types of user, in order.
    - First 3: `rwx` 
      - Owner's permissions.
      - By default, the user who created the file is the owner.
      - Here, all three permissions (read, write, execute) are set.
    - Second 3: `r-x`
      - The group's permissions.
      - Here, r and execute (not write).
    - Third 3: `r-x`
      - Any other user (not the owner or the group).

    - Source: [File Permissions blog](https://www.guru99.com/file-permissions.html)

## Input/Output Streams
Open Questions:
  - What exactly is a stream?
  - What's the difference between `|` and `<` in a bash script?
  - 
- The three standard streams are standard input (`stdin`), standard output (`stdout`), and standard error (`stderr`).
  - Each has an integer file descriptor associated with it:
    - 0: stdin
    - 1: stdout
    - 2: stderr

### Redirect `>`
- Redirects pass output to a file or stream.
  - `ls > capture.txt` redirects the output of `ls` to a file called `capture.txt`
    - if `capture.txt` does not exist, it will be created. If it does exist, it will be overwritten.
  - `ls >> capture.txt` does the same as above, but appends rather than overwrites.
- By default, `>` redirects stdout. But it can be used to redirect stderr with `2>`.
  - `./example_script.sh 2> capture.txt` redirects only stderr (using 2, the integer file descriptor for stderr).
    - Here, stdout would be printed to screen as normal, but any error messages get redirected to the capture.txt file.
    - `ls > capture.txt` is equivalent to `ls 1> capture.txt`
- Specify seperate files for stdout and stderr
  - `./example_script.sh 1> capture1.txt 2> capture2.txt`: stdout goes to capture1, stderr goes to capture2.
- Redirect both stdout and stderr to the same file:
  - `./example_script.sh &> capture.txt`

### Pipe `|`
- 

- [HowToGeek on Linux streams](https://www.howtogeek.com/435903/what-are-stdin-stdout-and-stderr-on-linux/)
- Linux Pocket Guide book
