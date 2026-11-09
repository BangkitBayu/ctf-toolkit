# Linux Cheatsheets

## Manipulation & Text Processing

| Command | Function                 | Example                     |
| ------- | ------------------------ | --------------------------- |
| echo    | output text              | echo "hello world"          |
| grep    | searches inside for text | grep "password123" file.txt |

## System Information & User

| Command | Function                     | Example |
| ------- | ---------------------------- | ------- |
| whoami  | output active user on system | whoami  |

## File Management & Directory

| Command | Function                       | Example                 |
| ------- | ------------------------------ | ----------------------- |
| ls      | list item in directory         | ls                      |
| cd      | Move into folder               | cd /folder_name         |
| cat     | show content of a file         | cat note.txt            |
| pwd     | print working directory        | pwd                     |
| find    | search for files by their name | find -name filename.txt |

## Shell Operator (Combining Command)

| Command | Function                                               | Example                         |
| ------- | ------------------------------------------------------ | ------------------------------- |
| &       | Run command without wait one of command done execution | whoami & cd folder              |
| &&      | Run both command with one by one execution             | cd /folder1 && cat note.txt     |
| >       | Redirect output with overwriting content               | echo "hello world" > hello.txt  |
| >>      | Redirect output without overwriting content            | echo "hello world" >> hello.txt |

## References

- Linux fundamental 1 - ![https://tryhackme.com/room/linuxfundamentalspart1](https://tryhackme.com/room/linuxfundamentalspart1)
- Linux fundamental 2 -
- Linux fundamental 3 -
