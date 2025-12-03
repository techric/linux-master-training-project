# Module 09: Introduction to Shell Scripting

## Overview

This module introduces shell scripting—a critical skill for cloud engineers, DevOps practitioners, and Linux administrators. Shell scripts automate tasks, reduce mistakes, and allow you to execute complex workflows consistently across servers.

You will learn how to create, run, and debug basic shell scripts using `bash`.

## Learning Objectives

By the end of this module, you should be able to:

- Create and run a simple shell script  
- Understand the `#!/bin/bash` shebang line  
- Use variables, conditionals, and loops  
- Accept user input and arguments  
- Use exit codes  
- Troubleshoot script execution issues  
- Apply scripting to real automation tasks  

---

## Lesson 1: What Is a Shell Script?

A shell script is a text file containing commands that the shell executes in order. Scripts are used for:

- Automating tasks  
- Deploying applications  
- Monitoring systems  
- Running backups  
- Setting up servers  

Scripts usually start with a **shebang** line:

    #!/bin/bash

This tells Linux which shell should run the script.

---

## Lesson 2: Creating Your First Script

1. Create a file:

       nano hello.sh

2. Add the following:

       #!/bin/bash
       echo "Hello, world!"

3. Save and exit.

4. Make the script executable:

       chmod +x hello.sh

5. Run it:

       ./hello.sh

---

## Lesson 3: Using Variables

Variables store values for reuse.

Example:

    #!/bin/bash
    NAME="Eric"
    echo "Hello, $NAME"

System variables (environment variables) are accessed the same way:

    echo $HOME
    echo $USER

---

## Lesson 4: Accepting User Input

Prompt the user:

    #!/bin/bash
    echo "What is your name?"
    read NAME
    echo "Welcome, $NAME"

Script arguments:

    ./script.sh arg1 arg2

Inside the script:

    #!/bin/bash
    echo "First argument: $1"
    echo "Second argument: $2"

Number of arguments:

    echo "You passed $# arguments"

---

## Lesson 5: Conditionals (if/else)

Example:

    #!/bin/bash
    if [ -f /etc/passwd ]; then
        echo "File exists"
    else
        echo "File not found"
    fi

Numeric comparison:

    if [ $1 -gt 10 ]; then
        echo "Greater than 10"
    fi

String comparison:

    if [ "$1" = "admin" ]; then
        echo "Welcome admin"
    fi

---

## Lesson 6: Loops

### For loop:

    #!/bin/bash
    for i in 1 2 3 4 5; do
        echo "Number: $i"
    done

### While loop:

    #!/bin/bash
    COUNTER=1
    while [ $COUNTER -le 5 ]; do
        echo "Counter: $COUNTER"
        COUNTER=$((COUNTER+1))
    done

---

## Lesson 7: Exit Codes

Commands return exit codes:

- `0` = success  
- non-zero = failure  

Check last exit code:

    echo $?

Example:

    cp file1 file2
    if [ $? -ne 0 ]; then
        echo "Copy failed!"
    fi

Exit from script:

    exit 1

---

## Lesson 8: Troubleshooting Shell Scripts

### Script won't run  
Missing execute bit.

    chmod +x script.sh

### “command not found”  
Incorrect shebang or typo.

### “Permission denied”  
Trying to write to restricted directories.

### Debugging with `bash -x`

Run script:

    bash -x script.sh

Shows each command as it executes.

---

## Lesson 9: Real Automation Examples

### Example: Log cleaner

    #!/bin/bash
    find /var/log -type f -name "*.log" -size +50M -delete

### Example: Service status monitor

    #!/bin/bash
    if systemctl is-active --quiet nginx; then
        echo "Nginx is running"
    else
        echo "Nginx is NOT running"
    fi

### Example: Backup script

    #!/bin/bash
    tar -czf backup.tar.gz /home/$USER

Automation scripts like these are common in real cloud engineering work.

---

## Lab Exercise

1. Create a script called `userinfo.sh`:

       nano userinfo.sh

2. Script should:

- Print username  
- Print home directory  
- Print current date  
- Print load average  

3. Make it executable:

       chmod +x userinfo.sh

4. Run it:

       ./userinfo.sh

5. Create another script that loops from 1 to 10 and prints each number.

6. Create a script that accepts two arguments and prints their sum.

Record anything confusing or unexpected.

---

## Quiz

1. What does the shebang line do?  
2. How do you make a script executable?  
3. What is the difference between `$1` and `$#`?  
4. What does an exit code of `0` mean?  
5. How do you debug a script?  

---

## Notes and Personal Observations

Use this area to record:

- Useful scripts you wrote  
- Errors you encountered  
- Concepts that need more review  
- Ideas for future automation tasks  
