---
title: "Bash Scripting"
date: 2025-03-11 00:00:00 +0800
categories: [DevOps, Scripting]
image: assets/images/bash-scripting.jpg
tags: [bash, shell, scripting, linux]
---

# Bash Scripting

3-  a file that contains linux commands
- To avoid the repetitive tasks like installations or logics or files or permissions
- Bulk actions
-sh (bourne shell) - /bin/sh
-bash (bourne again shell) - /bin/bash  --- improved version of sh


```
#!/bin/bash    (shebang line to decide the type of shell the user wants to use)

echo "setting up a server"

variable_name = value
variable_name = $(command)

echo "using file $variale_name to configure"

if [ -d 'config' ]
then
  echo "reading config directory contents "
  config_files = $(ls config)
else
  echo "config dir not found. Creating one"
  mkdir config
fi

```

-c file - Checks if file is a character special file; if yest then the condition becomes true.
-d file - Checks if file is a directory; if yes, then the condition becomes true.
-f file - Checks if file is an ordinary file as opposed to a directory or special file; if yes, then the condition becomes true.
-g file - Checks if file has its set group ID (SGID) bit set; if yes, then the condition becomes true.
-k file - Checks if file has its sticky bit set; if yes, then the condition becomes true.
-p file -Checks if file is a named pipe; if yes, then the condition becomes true.

### Giving the parameter while running 
```
user_group = $1
if [ "user_group" == "nana"
then
echo " configure the server"
elif ["$user_group" == "admin"]
then
echo "administer the server"
else 
echo "No permission to configure the server. Wrong user group"
fi
```

```
# ./setup.sh admin
```
### Explicitly give the user input

```
# ! / bin/ bash
echo " Reading user input "
read -p "Please enter your password:" user_pwd
echo "thanks for your password $user_pwd "
```


## Loop
```
for   while 
do    do
done  done
```


