# Shell

## Open questions
- What is symlinking?
- Binary vs script
- what are environment variables?
  https://www.digitalocean.com/community/tutorials/how-to-read-and-set-environmental-and-shell-variables-on-linux#:~:text=Environmental%20variables%20are%20used%20to,like%20the%20current%20working%20directory.
- Is a .rb file an executable?
  - How to turn a Ruby file into an executable: https://medium.com/@mbauza/executable-file-in-ruby-165ec254d564
    - https://www.vikingcodeschool.com/falling-in-love-with-ruby/running-ruby-scripts
  - answer: an rb file can be an executable if you a) change its permissions to allow exeuction and b) add shebang line to top of the file
    - alternatively, you can just run the file with ruby (ie, ruby your-filename.rb)

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
