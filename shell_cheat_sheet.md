## Directory navigation and organisation

- `cd target_directory`: change directory to `target_directory` which is either an absolute path of the form `/path/to/directory`or a relative path of the form `path/to/director`(note the `/` at the beginning)
- `cd ..`: move one directory up
- `cd ../..`: move two directories up
- `pwd`: print the present working directory
- `mkdir directory_to_be_created`: create the directory directory_to_be_created
- `mv <source> <target>` moves `<source>` to `<target>`
- `cp <source> <target>` copies `<source>` to `<target>`


## Directory contents 
- `ls`: list the content of the current directory
- `ls target_directory`: list the content of the target directory
- `ls -hl`: list the content in verbose and human-readable form inlcuding size and permission bits, etc. 
- `ls -A`: list also hidden files which start with a dot `.`, e.g., `.this_is_a_hidden_text_file.txt`
- `tree` displays the file and dirctory tree, depth is specified with the option `L`, e.g., `tree -L 4 ~`


## Searching

- `find`: find directories of files, e.g., `find . -type f -name *.py` search for all `.py` files in the current directory
- `grep`: find phrases in files, e.g., `grep -ir textvectorization llm.py` search for all (case-insensitive) occurrences of the patter `textvectorization` within the file `llm.py`


## Special characters

- Wild cards: `*` *Example:* `ls -hl *.pdf` (list all pdf files in the current directory)
- `.`, `..` *Example:* `ls .`, `cd ../data` (move to the directory `data` located one dir up)
- `\` escapes the special meaning of special characters. *Example*: `rm *` vs. `rm \*` (DO NOT TRY THIS unless you're absolutely certain what the difference is and you intend to execute the command `rm *`)
- `.filename`: a dot prefixing the filename indicates a hidden file
- `~` is the home directory of the current user
- `&&`: `CMD1 && CMD2` `CMD2` executed only after successful execution of `CMD1`  
- `||`: `CMD1 || CMD2` `CMD2` executed only after failed execution of `CMD1`
- `;`: `CMD1 ; CMD2` `CMD2` executed regardless of exit status of `CMD1`


## Piping commands, `xargs` 

- piping of commands with `|`, e.g., `ls | wc -l` counts files in a directory
- `xargs` 
- `find` with the option `-exec`. Sometimes you'll need to put ` \;` at the end. 


## General notes

- Hitting the tab key <kbd>&#8677;</kbd> while typing tries to complete the command or filepath etc. 
- Using the arrow key <kbd>&#8593;</kbd> shows the previously used commands; using <kbd>&#8593;</kbd> and 
  <kbd>&#8595;</kbd> navigates through them.
- The key combination <kbd>Strg</kbd>+<kbd>R</kbd> searches through the previously used commands.
- `man program_in_question` lists the `man` pages aka the manual and extensive help of the program `program_in_question`. Is exited with pressing `q`. 
- `program_in_question file_to_be_opened` opens the file `file_to_be_opened` with the program `program_in_question`
- Using curly brackets to create multiple directories in one go, e.g., `mkdir directory_{A,B}` creates `directory_A` and `directory_B`. 


## Use of braces, brackets, parentheses

| Brackets                                  |                                          |
|-------------------------------------------|------------------------------------------|
| `if [ CONDITION ]`                        | Test construct                           |
| `if [[ CONDITION ]]`                      | Extended test construct-                 |

| Curly Braces                              |                                          |
| ---------------------                     | ------------------------                 |
| `${variable}`                             | Parameter substitution                   |  
| `{ command1; command2; . . . commandN; }` | Block of code                            |  
| `{string1,string2,string3,...}`           | Brace expansion                          |  
| `{a..z}`                                  | Extended brace expansion                 |  
| `{}`                                      | Text replacement, after find and xargs   |

| Parentheses                              |                                          |
| ---------------------                     | ------------------------                 |
| `( command1; command2 )`                  | Command group executed within a subshell |  
| `Array=(element1 element2 element3)`       | Array initialization                     |  
| `result=$(COMMAND)`                        | Command substitution, new style          |  
| `>(COMMAND)`                               | Process substitution                     |  
| `<(COMMAND)`                               | Process substitution                     |

| Double Parentheses     |    |                   
| ---------------------                     | ------------------------|
| `(( var = 78 ))`                            | Integer arithmetic |   
| `var=$(( 20 + 5 ))`                         | Integer arithmetic, with variable assignment |   
| `(( var++ ))`                               | C-style variable increment |   
| `(( var-- ))`                               | C-style variable decrement |



## Variable naming
