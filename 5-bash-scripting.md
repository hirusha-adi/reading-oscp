# Bash Scripting

- have shebang at top

```bash
#!/bin/bash 
```

- need executable permissions to run

```
chmod +x file.sh
```

- variables
- use `$` infront of var name of access vars

```bash
greeting='Hello World'
echo $greeting # print 'Hello World' to console
```

- get username

```bash
user=$(whoami)
echo $user
```

- get command line arguments when executing the script

```bash
echo "The first two arguments are $1 and $2"

# > ./arg.sh hello there
# The first two arguments are hello and there
```

![alt text](image-1.png)


- prompts / user input

```
kali@kali:~$ cat ./input.sh
#!/bin/bash
echo "Hello there, would you like to learn how to hack: Y/N?"
read answer
echo "Your answer was $answer"

kali@kali:~$ chmod +x ./input.

kali@kali:~$ ./input.sh
Hello there, would you like to learn how to hack: Y/N?
Y
Your answer was Y
```

- prompts

```bash
read -p 'Username: ' username
# get input to same line
# store it to variable `username`

read -sp 'Password: ' password
# get input to same line
# dont show whats being entered to user
# store it to variable `password`
```

- if statements
- `-lt` means less than
- ![alt text](image-2.png)

```bash
#!/bin/bash
read -p "What is your age: " age
if [ $age -lt 16 ]
then
 echo "You might need parental permission to take this course!"
elif [ $age -gt 60 ]
then
 echo "Hats off to you, respect!"
else
 echo "Welcome to the course!"
fi

# What is your age: 65
# Hats off to you, respect!
```

- boolean logic
- `&&` and
- `||` or
- `==` is equal?

```bash
if [ $USER == 'kali' ] && [ $HOSTNAME == 'kali' ]
then
 echo "Multiple statements are true!"
else
 echo "Not much to see here..."
fi
```



