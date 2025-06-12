# Day 9 – Writing Basic Shell Scripts

## 🎯 Objectives

- Understand what a shell script is
- Learn how to create and execute basic shell scripts
- Use variables, conditionals, and loops in Bash scripts
- Understand script permissions and best practices

---

## 📝 What is a Shell Script?

A **shell script** is a text file containing a series of shell commands. It automates tasks that you would otherwise run one by one in a terminal.

> 📁 Shell scripts typically use the `.sh` extension and are written for `bash` or another shell.

---

## 🧱 Creating Your First Script

### Step 1: Create a new file

```bash
nano hello.sh

### Step 2: Add the following content
```bash
#!/bin/bash
echo "Hello, world!"

```

### Step 3: Make the script executable

```
chmod +x hello.sh
```

### Step 4: Run the script

```bash
./hello.sh

```
## 🧾 Script Structure

```bash
#!/bin/bash              # Shebang line: tells the system to use Bash
# This is a comment

echo "This is a Bash script"

```

## 🧮 Variables in Scripts

```bash
#!/bin/bash

name="Irfan"
echo "Welcome, $name!"

```
> No space before or after = when assigning values.

## 📘 Reading User Input
```bash
#!/bin/bash

echo "Enter your name:"
read username
echo "Hello, $username!"

```

## 🔀 Conditional Statements

### if–else Statement
```bash
#!/bin/bash

echo "Enter a number:"
read num

if [ $num -gt 10 ]; then
    echo "The number is greater than 10"
else
    echo "The number is 10 or less"
fi

```
> -gt = greater than, -lt = less than, -eq = equal to, -ne = not equal

## 🔁 Loops in Bash
### for loop
```bash
#!/bin/bash

for i in 1 2 3 4 5
do
    echo "Number: $i"
done

```
### while loop
```bash
#!/bin/bash

count=1
while [ $count -le 5 ]
do
    echo "Count is: $count"
    ((count++))
done

```

## 📂 Script Arguments
```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
```
Run it like:

```bash
./myscript.sh one two
```

## 🛑 Exit Status
### ✅ What is an Exit Status?
- After any command runs, it returns an exit code.

- The exit status can be accessed using:
```bash
echo $?

```

- Convention:

    - 0 → Success

    - Non-zero → Error or abnormal termination

### 🔢 Common Exit Codes and Their Meanings
| Exit Code | Meaning                           | Example Scenario                         |
| --------- | --------------------------------- | ---------------------------------------- |
| `0`       | Success (no error)                | Command/script executed successfully     |
| `1`       | General error                     | Syntax error, missing argument, etc.     |
| `2`       | Misuse of shell builtins          | Incorrect use of `exit`, `break`, etc.   |
| `126`     | Command invoked cannot execute    | File exists but isn’t executable         |
| `127`     | Command not found                 | Typo in command or command not installed |
| `128`     | Invalid exit argument             | Used `exit 9999` which exceeds limit     |
| `130`     | Script terminated by Ctrl+C       | User interruption (SIGINT)               |
| `137`     | Killed (usually by `kill -9`)     | Process forcefully terminated (SIGKILL)  |
| `255`     | Exit status out of range or error | Invalid return or forced abort           |

> ℹ️ Exit statuses are in the range 0–255.


### 🧪 Practical Examples

#### Example 1: Success (Exit 0)

```bash
#!/bin/bash
echo "Everything went fine"
exit 0
```

```bash
./script.sh
echo $?  # Output: 0
```

#### Example 2: General Error (Exit 1)
```bash
#!/bin/bash
echo "Something went wrong"
exit 1
```

```bash
./script.sh
echo $?  # Output: 1
```

#### Example 3: Command Not Found (Exit 127)

```bash
#!/bin/bash
fakecmd
```

```bash
./script.sh
echo $?  # Output: 127
```

#### Example 4: Script Not Executable (Exit 126)

```bash
chmod -x myscript.sh
./myscript.sh
echo $?  # Output: 126
```

#### Example 5: Killed by User (Exit 130)
Run a long process and press Ctrl+C:

```bash
#!/bin/bash
sleep 60
```

```bash
./script.sh
# Press Ctrl+C
echo $?  # Output: 130
```


### 🛠 How to Use Exit Status in Scripts

```bash
#!/bin/bash

ping -c 1 google.com > /dev/null

if [ $? -eq 0 ]; then
    echo "Internet is up"
    exit 0
else
    echo "Internet is down"
    exit 1
fi

```


## 🧪 Practice Tasks
1. Write a script to greet a user by name.

2. Write a script to add two numbers entered by the user.

3. Write a script that loops from 1 to 10 and prints each number.

4. Write a script that checks if a file exists and displays an appropriate message.

## 🛠️ Best Practices
- Always include the shebang line (#!/bin/bash)
- Comment your code
- Use descriptive variable names
- Check for user input and handle errors
- Make scripts executable: chmod +x script.sh

## 📚 Resources
- [Bash Scripting Cheat Sheet](https://devhints.io/bash)
- [ShellCheck – Shell script linter](https://devhints.io/bash)
- [Bash Guide for Beginners (The Linux Documentation Project)](https://devhints.io/bash)

