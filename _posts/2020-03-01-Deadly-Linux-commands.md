---
layout:     post
title:      Deadly Linux Commands 
date:       2020-03-01 00:00:00
author:     Manas kashyap
summary:    Some Linux Basics Commands that you should never use
categories: jekyll
thumbnail:  cli
tags:
 - Dangerous
 - linux
 - Command Line
 - Basics
---
<u>**Warning:** These commands should NEVER be executed. They will most likely destroy your system (or ruin a major part) before you can stop them, however, if you want to see how they work, you could run them inside a Virtual Machine.</u>

So, let's get started:

```
"rm -rf /"
```

This command basically means "remove all files (even Read Only files) recursively in the root (top) directory" (can also be written as "shred -rf /)"

```
":(){:|:&};:"

```

This command is known as a 'Fork Bomb'. It operates by defining a function called ':', which calls itself twice, once in the foreground and once in the background. It keeps on executing again and again till the system freezes.

```
"'command' > /dev/sda"
```

This command writes the output of 'command' to the specified drive. This is considered deadly because it overwrites any data on the drive.

```
"mkfs.ext3 /dev/sda"

```

This command is known as a format command. It will format the specified drive to an ext3 format, wiping everything on the drive.

```
"dd if=/dev/random of=/dev/sda"
```

This command writes random data onto the specified drive, and overwrites any data within that drive.
These are a few of the many deadly commands for Linux. 



*This post was made for the newcomers in Linux (and maybe some of the veterans) who are likely to run the commands if told it will fix a problem. I, myself, was tricked into executing the 1st command listed here a while back and it took me FOREVER to get all my data and documents back.* 

