# Linux Cheatsheets

## Manipulation & Text Processing

| Command | Function                 | Example                     |
| ------- | ------------------------ | --------------------------- |
| echo    | output text              | echo "hello world"          |
| grep    | searches inside for text | grep "password123" file.txt |
| cat     | show content of a file   | cat note.txt                |

## System Information & User

| Command | Function                     | Example |
| ------- | ---------------------------- | ------- |
| whoami  | output active user on system | whoami  |

## File Management & Directory

| Command | Function                       | Example                                     |
| ------- | ------------------------------ | ------------------------------------------- |
| ls      | list item in directory         | ls                                          |
| cd      | Move into folder               | cd /folder_name                             |
| pwd     | print working directory        | pwd                                         |
| find    | search for files by their name | find -name filename.txt                     |
| cp      | Copy                           | cp name target_directory                    |
| mv      | Move or rename                 | mv file /path-to-move, mv old_name new_name |

## System Information and Help Commands

| Command | Function                    | Example |
| ------- | --------------------------- | ------- |
| man     | show docummentation command | man cat |

## Shell Operator (Combining Command)

| Command | Function                                               | Example                         |
| ------- | ------------------------------------------------------ | ------------------------------- |
| &       | Run command without wait one of command done execution | whoami & cd folder              |
| &&      | Run both command with one by one execution             | cd /folder1 && cat note.txt     |
| >       | Redirect output with overwriting content               | echo "hello world" > hello.txt  |
| >>      | Redirect output without overwriting content            | echo "hello world" >> hello.txt |

## References

- Linux fundamental 1,2,3 - [https://tryhackme.com/module/linux-fundamentals](https://tryhackme.com/module/linux-fundamentals)
