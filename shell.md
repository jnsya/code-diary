# Shell

## Open questions
- What is symlinking?
- what are environment variables?
  -   https://www.digitalocean.com/community/tutorials/how-to-read-and-set-environmental-and-shell-variables-on-linux#:~:text=Environmental%20variables%20are%20used%20to,like%20the%20current%20working%20directory.
- What are binstubs in a rails project?
  - Why do we need bundle exec?

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
