# Module 03: Linux File Permissions and Ownership

## Overview

This module teaches you how Linux manages file permissions, ownership, and access control. Permissions are essential for security, troubleshooting, and system administration. As a cloud engineer, many issues you face will come down to incorrect permissions, missing execute bits, or files owned by the wrong user.

Understanding permissions is one of the most important parts of becoming Linux-competent.

## Learning Objectives

By the end of this module, you should be able to:

- Read and interpret Linux file permission strings  
- Understand user, group, and other access levels  
- Change permissions using `chmod`  
- Change file ownership using `chown` and `chgrp`  
- Understand symbolic vs. numeric modes (e.g., `chmod 755`)  
- Identify and fix common permission errors  

---

## Lesson 1: How Linux Permissions Work

Every file and directory in Linux has three permission sets:

- **User (u)** — the owner  
- **Group (g)** — members of the file’s group  
- **Other (o)** — everyone else  

Each set can have:

- **r** — read  
- **w** — write  
- **x** — execute (or “enter” for directories)

You can view permissions using:

    ls -l

Example output:

    -rw-r--r-- 1 ec2-user ec2-user  120 Jan 10 10:42 notes.txt

Breaking down the permission string:

    - rw- r-- r--
      |   |   |
      |   |   └─ permissions for others
      |   └──── permissions for group
      └──────── permissions for user (owner)

The first character (`-`, `d`, `l`) indicates file type.

---

## Lesson 2: Changing Permissions with chmod

`chmod` changes permissions.  
There are **two ways** to use it: symbolic mode and numeric mode.

### Symbolic Mode

Add execute for the user:

    chmod u+x script.sh

Remove write for group:

    chmod g-w file.txt

Give everyone execute permission:

    chmod a+x deploy.sh

### Numeric Mode

Permissions are encoded as:

- read (4)
- write (2)
- execute (1)

Add up the values for each group.

Common examples:

    chmod 644 file.txt     # owner: rw-, group: r--, others: r--
    chmod 600 file.txt     # owner only
    chmod 755 script.sh    # owner: rwx, group: r-x, others: r-x
    chmod 700 secure.sh    # owner only, full access

Numeric mode is faster once you understand it.

---

## Lesson 3: Changing Ownership with chown and chgrp

Change the owner:

    chown ec2-user file.txt

Change owner and group:

    chown ec2-user:developers file.txt

Change only the group:

    chgrp developers file.txt

Change ownership recursively (dangerous if misused):

    chown -R ec2-user:ec2-user /var/www

Ownership issues are extremely common when deploying applications or working with shared folders.

---

## Lesson 4: Permissions and Directories

Directory permissions work slightly differently:

- **r** = view contents  
- **w** = create or delete files inside  
- **x** = enter the directory (“search”)

Example:

A directory that looks like:

    drwxr-x---

Means:

- Owner: read, write, execute  
- Group: read, execute  
- Other: no access  

To make a directory accessible:

    chmod 755 myfolder

To secure a directory completely:

    chmod 700 secrets

---

## Lesson 5: Common Permission Errors and Fixes

### “Permission denied”

Caused by missing execute or write permissions.

Fix:

    chmod +x file
    chmod u+w directory

### “Operation not permitted”

You are not the owner.

Fix (if you have sudo):

    sudo chown $USER file

### Script won’t run even though the file exists

Missing execute bit.

Fix:

    chmod +x script.sh

### Nginx/Apache cannot read a file

The web server user does not have permission.

Fix:

    chown root:nginx /var/www/html
    chmod 755 /var/www/html

### Application cannot write to a directory

Missing write permission.

Fix:

    chmod 775 directory
    chown -R user:group directory

---

## Lab Exercise

1. Create a new file:

       touch perm_test.txt

2. View its permissions:

       ls -l perm_test.txt

3. Remove read permission for others:

       chmod o-r perm_test.txt

4. Add execute permission for the user:

       chmod u+x perm_test.txt

5. Create a directory:

       mkdir secure_dir

6. Restrict it so only you can access it:

       chmod 700 secure_dir

7. Create a file inside the directory and verify permissions:

       touch secure_dir/inside.txt
       ls -l secure_dir

8. Change the owner and group of the folder (use a group you actually have):

       chown $USER:$USER secure_dir

Record anything unexpected.

---

## Quiz

1. What do the three permission groups (user, group, other) represent?  
2. What does the permission string `rwxr-xr--` mean?  
3. What does `chmod 755` do?  
4. What is the difference between `chown` and `chgrp`?  
5. Why is `chmod -R` potentially dangerous?  

---

## Notes and Personal Observations

Use this section to record:

- Permission combinations you want to remember  
- Any mistakes you made  
- How you fixed permission errors  
- Questions for later review
