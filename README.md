How to setup jupyter notebook environment for the bash commands:

```bash
conda create -p "ENVIRONMENT_PATH" python=3.11 notebook bash_kernel
conda activte "ENVIRONMENT_PATH"
python -m bash_kernel.install
```

COPYRIGHT
This repository has been cloned from [Terminal Course](https://gitlab.com/courses_pub/inf_ds_ws2425/-/tree/master/terminal_course?ref_type=heads)

CONTENTS
========

- `session_01.md`: prerequisites, first steps, piping, exercises 
- `session_02.md`: globbing, wildcards, `for` loops, `if` statements, `find`, `grep`


USED COMMANDS, CHEAT SHEET
==========================

The file `used_commands.txt` contains all bash commands either used in the text or needed for exercises. Also, you can find a bash cheat sheet `linux_cheat_sheed.md`. Do extend this cheat sheet with your own commands! 


REFERENCES
==========

Many exercises are copied from the fantastic course [./missing semester](https://missing.csail.mit.edu/2020/). Also, the session contents are heavily inspired by this course and quote it sometimes verbatim. We omit references in the sessions, but state their existence here. 

Other resources on specific topics are: 
- For loops and conditionals: https://www.shellscript.sh/
- Use of braces/brackets: https://stackoverflow.com/questions/2188199/how-to-use-double-or-single-brackets-parentheses-curly-braces
- Over the wire: https://overthewire.org/wargames/bandit/
- Shakespeare's work: https://raw.githubusercontent.com/brunoklein99/deep-learning-notes/master/shakespeare.txt
- sed command in Linux: https://www.howtogeek.com/666395/how-to-use-the-sed-command-on-linux/
- file permission notation: https://en.wikipedia.org/wiki/File-system_permissions#Notation_of_traditional_Unix_permissions
- piping vs. xargs: https://stackoverflow.com/questions/35589179/when-to-use-xargs-when-piping


