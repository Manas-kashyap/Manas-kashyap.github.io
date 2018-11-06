---
layout:     post
title:      Basic Linux Commands -1
date:       2018-11-06 11:40:00
author:     Manas kashyap
summary:    Some Basics for Linux Command Line - part 1
categories: jekyll
thumbnail:  cli
tags:
 - linux
 - Command Line
 - Basics
---
In this Blog, I am going to tell 10 Basic Linux Commands. 
This Blog is part of a series that is going to be released soon . 

## What is Linux? 
Linux is best known as the most used open source operating system.

We use the term “Linux” to refer to the Linux kernel, but also the set of programs, tools, and services that are typically bundled together with the Linux kernel to provide all of the necessary components of a fully functional operating system.

[Why to use linux?][1]


## What is Linux Command Line ,shell or Terminal ? 

A terminal, shell is a program that receives commands from the user and gives it to the OS to process, and it shows the output.
Linux's shell is its main part. Its distros come in GUI (graphical user interface), but basically, Linux has a CLI (command line interface). In this tutorial, we are going to cover the basic commands that we use in the shell of Linux..

##### To open the terminal, press Ctrl+Alt+T in Ubuntu, Debian, or press Alt+F2, type in gnome-terminal, and press enter. In Raspberry Pi, type in lxterminal. There is also a GUI way too by searching for terminal in start menu. 

## Linux Commands

**1) ls** - Use the **"ls"** command to know what files are in the directory you are in. You can see all the hidden files by using the command ***“ls -a”***.

**2) cd** - Use the **"cd"** command to go to a directory. For example, if you are in the home folder, and you want to go to the downloads folder, then you can type in “***cd Downloads”***. Remember, this command is case sensitive, and you have to type in the name of the folder exactly as it is. 
But there is a problem with these commands. Imagine you have a folder named “Raspberry Pi”. In this case, when you type in “cd Raspberry Pi”, the shell will take the second argument of the command as a different one, so you will get an error saying that the directory does not exist. Here, you can use a backward slash. That is, you can use **“cd Raspberry\ Pi”** in this case. Spaces are denoted like this: *If you just type “cd” and press enter, it takes you to the home directory.* To go back from a folder to the folder before that, you can type **“cd ..”** . The two dots represent back.

**3) mkdir & rmdir** - the command when you need to create a folder or a directory. For example, if you want to make a directory called “freshers”, then you can type “*mkdir freshers”*.
Remember, as told before, if you want to create a directory named ***“fresher 2k18”***, then you can type *“**mkdir freshers\ 2k18**”*. 
Use **rmdir** to delete a directory. But *rmdir* can only be used to delete an empty directory. 
*To delete a directory containing files, use rm*.

**4) rm** - This command is used to delete files and directories.  Use **"rm -r*"** to delete just the directory. It deletes both the folder and the files it contains when using only the rm command.

##### Warning:- never do , "sudo rm -rf /*", it will delete all the files in your root directory leading to shutting down of system.

**5) touch** - This command is used to create a file. It can be anything, from an empty txt file to an empty zip file. For example, “touch new.txt”.

**6) man & --help** - To know more about a command and how to use it, use the man command. It shows the manual pages of the command. For example, *“man cd”* shows the manual pages of the cd command. Typing in the command name and the argument helps it show which ways the command can be used *(e.g., cd –help).*

**7) cp** - Use the *cp* command to copy files through the command line. It takes two arguments: *The first is the location of the file to be copied, the second is where to copy*.

**8) mv** - Use the mv command to move files through the command line. *We can also use the mv command to rename a file*. For example, if we want to rename the file “text” to “new”, we can use “mv text new”. It takes the two arguments, just like the cp command.

**9) cat** - Use the cat command to display the contents of a file. It is usually used to easily view programs.

**10) uname** - Use uname to show the information about the system your Linux distro is running. Using the command “uname -a” prints most of the information about the system. This prints the kernel release date, version, processor type, etc.

**<u>Tips and Tricks for Using Linux Command Line</u>**

You can use the clear command to clear the terminal . 

TAB can be used to fill up in terminal. For example, You just need to type “cd Doc” and then TAB and the terminal fills the rest up and makes it “cd Documents”.

Ctrl+C can be used to stop any command in terminal safely. If it doesn't stop with that, then Ctrl+Z can be used to force stop it.

You can exit from the terminal by using the exit command.

You can power off or reboot the computer by using the command sudo halt and sudo reboot.

##### For more commands visit [here][2].
These are some commands that will help you while starting in Linux terminal . 

Hopefully it helps you . 

[1]: https://manas-kashyap.github.io/jekyll/2018/11/03/Why-linux/
[2]: https://ss64.com/bash/
