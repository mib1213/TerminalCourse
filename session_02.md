## Basics of shell scripting

We know about executing commands in the shell and piping them together. But what about control flow expressions like conditionals or loops? These can be used in shell scripts. A few basics are good to know. 
- When launching scripts, you will often want to provide arguments that are similar. Bash has ways of making this easier, expanding expressions by carrying out filename expansion. These techniques are often referred to as shell *globbing*.
  - *Wildcards*: Whenever you want to perform some sort of wildcard matching, you can use `?` and `*` to match one or any amount of characters respectively. For instance, given files `foo`, `foo1`, `foo2`, `foo10`, and `bar`, the command `rm foo?` will delete `foo1` and `foo2` whereas `rm foo*` will delete all but `bar`.
  - *Curly braces* `{}`: Whenever you have a common substring in a series of commands, you can use curly braces for bash to expand this automatically. This comes in very handy when moving or converting files. Check out the examples:

>     convert image.{png,jpg}
>     # Will expand to
>     convert image.png image.jpg
>     
>     cp /path/to/project/{foo,bar,baz}.sh /newpath
>     # Will expand to
>     cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath
>     
>     # Globbing techniques can also be combined
>     mv *{.py,.sh} folder
>     # Will move all *.py and *.sh files
>     
>     
>     mkdir foo bar
>     # This creates files foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h
>     touch {foo,bar}/{a..h}
>     touch foo/x bar/y

- To assign variables in bash, use the syntax `foo=bar` and access the value of the variable with `$foo`. (Sometimes you'll notice brackets after the `$` like `$((foo))` or `${foo}`. These have a special meaning: see [here](https://stackoverflow.com/questions/2188199/how-to-use-double-or-single-brackets-parentheses-curly-braces)) Note that `foo = bar` will not work since it is interpreted as calling the foo program with arguments `=` and `bar`. In general, in shell scripts the space character will perform argument splitting. 
- Strings in bash can be defined with `'` and `"` delimiters, but they are not equivalent. Strings delimited with `'` are literal strings and will not substitute variable values whereas `"` delimited strings will.

>     my_machine:~$ foo=bar
>     echo "$foo"
>     # prints bar
>     my_machine:~$ echo '$foo'
>     # prints $foo

- Besides piping, there are more options to use different commands on the same line: `&&`, `||`, `;`

>     false || echo "Oops, fail"
>     # Oops, fail
>     
>     true || echo "Will not be printed"
>     #
>     
>     true && echo "Things went well"
>     # Things went well
>     
>     false && echo "Will not be printed"
>     #
>     
>     true ; echo "This will always run"
>     # This will always run
>     
>     false ; echo "This will always run"
>     # This will always run

- Sometimes you want to use the output of one command as variable, e.g., in `echo "Starting program at $(date)"`. This is achieved with round parentheses `$(CMD)`. Another example would be in the simple `for` loop: `for file in $(ls); do echo $file; done`. Do you understand what's going on? More on the use of brackets, you can find again [here](https://stackoverflow.com/questions/2188199/how-to-use-double-or-single-brackets-parentheses-curly-braces). 

- `for` loops have the following syntax:

>     #!/bin/sh
>     for i in 1 2 3 4 5
>     do
>        echo "Looping ... number $i"
>     done

Notice how the iteration variable `i` has to be prefixed with `$i` when accessing it. The iterable can be replaced with many other formulations. Try replacing it with the following: `$(ls)`, `{a..d}`, `{1..10}`, etc. Also, with the help of `;` the different lines of the loop can be put on one single line and run in the shell directly. E.g.: 

>     my_machine:~$ for num in {2..9}; do echo $num; done 

- When employing `if` statements, you should know that you are actually using a program called `test` or `[`. Upon remembering that arguments in a shell are separated by a space ` `, one can then deduce where to put spaces, namely around all the arguments of the program `[`:  

>     #!/bin/sh
>     foo="bar"
>     if [ $foo = "bar" ]
>     then
>        echo $foo
>     fi

Note that `=` is used for string comparison. For integers, one would use `-eq` as in  

>     my_machine:~$ if [ 3 -eq $((2+1)) ]; then echo 'True'; fi

For more comparison operators, it's very helpful to check `man [`!

- Now, that the amount of commands we used is growing, here are a couple of tricks in order to access commands that you have used before: 
   - Hitting the arrow key up <kbd>&#8593;</kbd> shows the last used command, using it multiple times the commands used before that.
   - The key combination <kbd>Strg</kbd>+<kbd>R</kbd> searches through the previously used commands.
   - `history | grep PATTERN` searches for `PATTERN` within your command history. 

- Often, you have to find files whose names match a certain pattern, but you don't know exactly which pattern. Or you're looking for directories only. For this, one uses `find`:

>       # Find all directories named src
>       find . -name src -type d
>       # Find all python files that have a folder named test in their path
>       find . -path '*/test/*.py' -type f
>       # Find all files modified in the last day
>       find . -mtime -1
>       # Find all zip files with size in range 500k to 10M
>       find . -size +500k -size -10M -name '*.tar.gz'

- Beyond listing files, `find` can also perform actions over files that match your query. This property can be incredibly helpful to simplify what could be fairly monotonous tasks.

>       # Delete all files with .tmp extension
>       find . -name '*.tmp' -exec rm {} \;
>       # Find all PNG files and convert them to JPG
>       find . -name '*.png' -exec convert {} {}.jpg \;

Note the ` \;` at the end of the commands. One often forgets this ;). Instead of `-name` one can also use case-insensitive pattern matching with `-iname`.

- `grep` searches also within files. E.g. `grep -ir foo .` searches recursively (`-r`) for all occurrences of the substring `foo` in all files in the current directory in a case-insensitive way (`-i`). Try it! 



## Exercises

1. Read `man ls` and write an `ls` command that lists files in the following manner
   - Includes all files, including hidden files
   - Sizes are listed in human-readable format (e.g. `454M` instead of `454279954`)
   - Files are ordered by recency
   - Output is colorized
2. Predict the outcome of the following code: 

>     #!/bin/sh
>     for i in hello 1 * 2 goodbye 
>     do
>        echo "Looping ... i is set to $i"
>     done

Make sure that you understand what is happening here. Try it without the `*` and try it again with the `*` in place. Try it also in different directories, and with the `*` surrounded by double quotes and surrounded by single quotes, and try it preceded by a backslash (`\*`), and also single quotes plus backslash as well as double quotes plus backslash.
3. Write a short `for` loop that counts all files (including hidden files) in the current directory. Check your result with the help of piping and `wc`! 
4. Write a short `for` loop that renames all files (hidden files excluded) in a directory of your choice (but not the directory `terminal_course`!) in such a way that all spaces are replaced by underscores `_`. 
5. Say you have a command that fails rarely. In order to debug it you need to capture its output, but it can be time-consuming to get a failure run. Write a bash script that runs the following script until it fails and captures its standard output and error streams to files and prints everything at the end. Bonus points if you can also report how many runs it took for the script to fail.

>     #!/usr/bin/env bash
>     n=$(( RANDOM % 100 ))
>     if [[ n -eq 42 ]]; then
>        echo "Something went wrong"
>        >&2 echo "The error was using magic numbers"
>        exit 1
>     fi
>     echo "Everything went according to plan"

6. Check out the program `dir_tree_gen`.
   1. Make sure it is in the same directory as `data`. Then, execute it (how?). What does it do?
   2. After having finished i.:
       - How many directories does `topdir` contain? 
       - How many `txt` files does `topdir` contain?
       - Which of these `txt` files contain the string `love` most often?
   3. After having finished ii.:
       - Move all `txt` files below `topdir` into `topdir` with a one-line command.
       - Rename all `txt` files in `topdir`now in such a way, that the file count is put between the filename and the extension, separated by `_`. Use zeros as padding, i.e., if there are a 3-digit number of files, the file counts should also be 3-digit. E.g.: `4b17d6.txt` becomes `4b17d6_023.txt`. 
       - Put today's date in the form `YYYYMMDD` at the beginning of each filename, separated by an underscore `_` from the original filename. E.g.: `4b17d6_020.txt` becomes `20240925_4b17d6_020.txt`. *Hint*: `for` loop
       - How can you concatenate all `txt` files into one `txt` file named `concat_shakespeare.txt`? Is it the same as `data/shakespeare.txt`?
7. Download the works of Shakespeare again (see Session 01). 
   1. Encrpyt it using `gpg`. Choose the symmetric algorithm AES256. 
   2. Now, examine the encrypted file with `file`. Open it with the terminal. First try `less`, then try `xxd`. 
   3. Count the occurence of the 16 hex digits, and compute their relative frequency. *Hint*: Use `xxd`, `fold`, `sort`, `uniq`
   4. Run a hypothesis test whether these 16 hex digits are distributed uniformly.
   5. Do the same with the unencrypted text file. 
8. Do exercise 7, but this time with public key cryptography.
   1. Generate an RSA key pair (4096 bits). 
   2. Use the public key to encrypt the file `shakespeare.txt`. 
   3. Run the same statistical analysis as above. 
