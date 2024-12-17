## Prerequisites

For this course, you need to be using a Unix shell like Bash or ZSH. If you are on Linux or macOS, you don’t have to do anything special. If you are on Windows, you need to make sure you are not running `cmd.exe` or PowerShell; you can use [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/) or a Linux virtual machine to use Unix-style command-line tools. To make sure you’re running an appropriate shell, you can try the command `echo $SHELL`. If it says something like `/bin/bash` or `/usr/bin/zsh`, that means you’re running the right program. 

No other prerequisites are required. 

## First steps with the terminal

- What you see first, when you open the terminal, is the prompt. You see the name of the machine you're on, the present working directory (`~` means your home directory), and the user. Try the commands `date` and `echo hello`. 
- Whitespaces usually separate *commands* from *arguments*. If this is not what you want, e.g., when accessing the directory `My Photos`, the respective phrase needs to be surrounded by quotes: either `'My Photos'` of `"My Photos"`. Sometimes the whitespace is also escaped with `\` (backslash): `My\ Photos`
- In order to execute commands, the shell needs to know which programs are referenced by the commands. Try `echo $PATH`, compare it to `which echo`, and then use this output in combination with `$PATH`. 
- For navigating directories, useful commands include `cd`, `pwd`, `realpath`. Displaying directory contents is done with `ls`, `ls -hl` for file permissions, `ls -A` and hidden files. Helpful special charactes are `~`, `.` (2x), `..`
- Options of `<command>` are usually explained in the `man` pages accessed via `man <command>`. `-` for one-letter option, `--` for words as option (`ls -h` vs. `ls --help`). 
- File permissions show up with `ls -l` and can be changed with `chmod`. Why is this good? 
- Moving files happens with `mv`, copying with `cp` (`cp -r` for directories; `r` stands for recursive very often). Directories are created with `mkdir`
- Standard input for termial programs is the keyboard, standard output text in the console. But one can also direct the text output of one program as input to another program:

>     my_machine:~$ echo hello > hello.txt
>     my_machine:~$ cat hello.txt 
>     hello 
>     my_machine:~$ cat < hello.txt 
>     hello 
>     my_machine:~$ cat < hello.txt > hello2.tx 
>     my_machine:~$ cat hello2.txt 
>     hello

- Another and even more powerful way to connect two programs is called *piping* and achieved with `|`. The `|` operator lets you “chain” programs such that the output of one is the input of another: 

>     my_machine:~$ ls -l / | tail -n1
>     drwxr-xr-x 1 root  root  4096 Jun 20  2019 var
>     my_machine:~$ curl --head --silent google.com | grep --ignore-case content-length | cut --delimiter=' ' -f2
>     219

- Sometimes the output of one commands is needed as an *argument* for another command. Then `xargs` is the way to go. See the following example: 

>     my_machine:~$ echo "hello_world_content" > "hello_world.txt"
>     my_machine:~$ ls | grep "hello"
>     hello_world.txt
>     my_machine:~$ ls | xargs grep "hello"
>     hello_world_content


## Exercises 

1. Create a new directory called `terminal_course` under `/tmp`.
2. Look up the `touch` program. The `man` program is your friend.
3. Use `touch` to create a new file called `wikipedia_head` and copy it to `/tmp/terminal_course`.
4. Delete the original file `wikipedia_head`. 
5. Suppose a directory contains the two folders `src` and `dest`. Predict the outcome of the following two commands: `mv src dest` vs. `mv src dest/`
6. Rename the file `wikipedia_head` in `/tmp/terminal_course` to `wiki_head.sh`.
7. One can use also `>>` to write to files. Can you find out the difference to `>`? 
8. Write the following into that file, one line at a time:

>     #!/bin/sh
>     curl --head --silent https://en.wikipedia.org

The first line might be tricky to get working. It’s helpful to know that `#` starts a comment in Bash, and `!` has a special meaning even within double-quoted (`"`) strings. Bash treats single-quoted strings (`'`) differently: they will do the trick in this case. See the [Bash quoting](https://www.gnu.org/software/bash/manual/html_node/Quoting.html) manual page for more information.

9. Try to execute the file, i.e., type the path to the script (`./wiki_head.sh`)
   into your shell and press enter. Understand why it doesn’t work by consulting the output of `ls` (*hint*: look at the permission bits of the file).
10. Run the command by explicitly starting the `sh` interpreter, and giving it the file `wiki_head.sh` as the first argument, i.e. `sh wiki_head.sh`. Why does this work, while `./wiki_head.sh` didn’t?
11. Look up the `chmod` program (e.g. use `man chmod`). You also might find [this](https://en.wikipedia.org/wiki/File-system_permissions#Notation_of_traditional_Unix_permissions) useful. 
12. Use `chmod` to make it possible to run the command `./wiki_head.sh` rather than having to type `sh wiki_head.sh`. How does your shell know that the file is supposed to be interpreted using `sh`? See [this page](https://en.wikipedia.org/wiki/Shebang_(Unix)) on the shebang line for more information.
13. Use `curl`, `|`, and `>` to write the “last modified” date output by `wiki_head` into a file called `last-modified.txt` in your home directory.
14. Use `|`, `grep`, and `wc -l`, in order to find out how often the word 'thou' (NOT words that contain 'thou') can be found in Shakespeare's work. *Hint*: Check [this](https://raw.githubusercontent.com/brunoklein99/deep-learning-notes/master/shakespeare.txt) out! 
15. Display lines 1789-1815 from the Shakespeare file using two different methods: `head` and `tail` as well as `sed`. *Hint*: Check their `man` pages. 
16. Assume `echo "hello_world_content" > "hello_world.txt"` has been run. Predict the output and understand their differences in the following command pairs: 
    - `ls | echo` vs. `ls | xargs echo`
    - `ls | cat` vs. `ls | xargs cat`
    - `echo "this_file_does_not_exists.txt" | cat` vs. `echo "this_file_does_not_exists.txt" | xargs cat`
17. Write a command that reads out your laptop battery’s power level or your desktop machine’s CPU temperature from `/sys`. *Note*: if you’re a macOS user, your OS doesn’t have `sysfs`, so you can skip this exercise.
