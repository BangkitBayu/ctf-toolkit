# Linux Cheatsheets

## Manipulation & Text Processing

| Command | Description                                              | Example                       |
| ------- | -------------------------------------------------------- | ----------------------------- |
| echo    | output text                                              | echo "hello world"            |
| grep    | searches inside for text                                 | grep "password123" file.txt   |
| cat     | show content of a file                                   | cat note.txt                  |
| sed     | change text without open the file                        | sed 's/old_w/new_w/' file.txt |
| awk     | process, filter, extract, manipulation data line by line | awk '{print $2, $3}' file.txt |

## System Information & User

| Command | Description                  | Example |
| ------- | ---------------------------- | ------- |
| whoami  | output active user on system | whoami  |

## File Management & Directory

| Command | Description                    | Example                                     |
| ------- | ------------------------------ | ------------------------------------------- |
| ls      | list item in directory         | ls                                          |
| cd      | Move into folder               | cd /folder_name                             |
| pwd     | print working directory        | pwd                                         |
| find    | search for files by their name | find -name filename.txt                     |
| cp      | Copy                           | cp name target_directory                    |
| mv      | Move or rename                 | mv file /path-to-move, mv old_name new_name |
| rm      | remove file or folder          | rm -f file.txt                              |

## User Management & Permission

| Command     | Description            | Example            |
| ----------- | ---------------------- | ------------------ |
| groupadd    | add new group          | groupadd demo      |
| groups      | list all groups        | groups             |
| usermod -aG | append user into group | usermod -aG tester |

## System Information and Help Commands

| Command | Description                 | Example |
| ------- | --------------------------- | ------- |
| man     | show docummentation command | man cat |

## Shell Operator (Combining Command)

| Command | Description                                            | Example                         |
| ------- | ------------------------------------------------------ | ------------------------------- |
| &       | Run command without wait one of command done execution | whoami & cd folder              |
| &&      | Run both command with one by one execution             | cd /folder1 && cat note.txt     |
| >       | Redirect output with overwriting content               | echo "hello world" > hello.txt  |
| >>      | Redirect output without overwriting content            | echo "hello world" >> hello.txt |

## References

- Linux fundamental 1, 2, 3 - [https://tryhackme.com/module/linux-fundamentals](https://tryhackme.com/module/linux-fundamentals)
- All about linux - [https://labex.io/linuxjourney](https://labex.io/linuxjourney)
- Linux group - [https://www.redhat.com/en/blog/linux-groups](https://www.redhat.com/en/blog/linux-groups)
