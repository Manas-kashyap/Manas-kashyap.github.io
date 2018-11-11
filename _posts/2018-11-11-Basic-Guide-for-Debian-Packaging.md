---
layout:     post
title:      Basic Guide for Debian Packaging
date:       2018-11-11 09:30:00
author:     Manas kashyap
summary:    A small peek into the Big world of Debian Packaging and how to start 
categories: jekyll
thumbnail:  debian
tags:
 - linux
 - Debian
 - Packaging
 - Open source
---
In this Blog, I am going to give a small peek in the vast world of Debian Packaging , and how to start with and why i do packaging . 

So , Lets Start !! 

## What is Debian ? 

[Debian][1] is an universal Linux based operating system . 

Debian systems currently use the [Linux](https://www.kernel.org/) kernel or the [FreeBSD](https://www.freebsd.org/) kernel.

The need of people is application software: programs to help them get what they want to do done, from editing documents to running a business to playing games to writing more software. Debian comes with over 51000 [packages](https://www.debian.org/distrib/packages) (precompiled software that is bundled up in a nice format for easy installation on your machine), a package manager (APT), and other utilities that make it possible to manage thousands of packages on thousands of computers as easily as installing a single application. All of it [free](https://www.debian.org/intro/free).

*It's like a tower. At the base is the kernel. On top of that are all the basic tools. Next is all the software that you run on the computer. At the top of the tower is Debian carefully organising and fitting everything so it all works together.*

**What is an Operating system ?** 

An operating system is the set of basic programs and utilities that make your computer run. 
At the core of an operating system is the kernel. The kernel is the most fundamental program on the computer and does all the basic housekeeping and lets you start other programs.

[Still thinking why to use linux ??](https://manas-kashyap.github.io/jekyll/2018/11/03/Why-linux/)

## What is meant by Debian Packaging?

Ever done *sudo apt install (anything)* ??

Wondered how apt install works and what file it fetches and how the file gets auto installed ?? 
So , here comes the Debian Packaging task. 
The aim of packaging is to allow the automation of installing, upgrading, configuring, and removing computer programs for Debian in a consistent manner.

*To be simple , its like making .deb of any software or module like in windows we have .exe file*

By doing Debian packaging we make Debian packages , which is a collection of files that allow for applications or libraries to be distributed via the Debian package management system. 

[More info](https://wiki.debian.org/Packaging)

#### <u>Enough of this  knowledge , get me to the coding part and lets do the  packaging</u>

##  Lets start with Prerequisites needed to do the work . 

So , To start with Debian packaging , we need a [Debian SID](https://www.debian.org/releases/sid/) environment  . 

*Oops , i have an ubuntu/arch/gentoo/mac machine and installing another OS , nah , not ready for a new OS* - its thats what you are thinking now , don't worry we have [Docker](https://www.docker.com/) , [lxc](https://linuxcontainers.org/lxc/introduction/) or [virtual machine](https://www.virtualbox.org/)   for you . 

See instructions given below to setup Debian Sid.

- LXC - <https://wiki.debian.org/Packaging/Pre-Requisites#LXC>
- Docker - <https://wiki.debian.org/Packaging/Pre-Requisites#Docker>
- Virtual Machine - <https://wiki.debian.org/Packaging/Pre-Requisites#Virtual_Machine>

I personally prefer Docker, and when i started with Debian packaging , i started with Docker container. 

##### Tips : for Linux users , who uses apt package manager , to install docker is now really easy , just do "sudo apt install docker.io"

You can go with the above setup link or the way i installed . 

**My way of installation of Docker SID environment** 

|                       Work to be done                        |                             Code                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| Pull Debian Sid image from docker hub using the following command |                    docker pull debian:sid                    |
|       Create a container with it and start bash on it        | docker run --privileged --name "sid" -it debian:sid /bin/bash |
|      Update and upgrade to latest versions of packages       |              apt-get update && apt-get upgrade               |

Exit after your work is done.

 If you need to connect to it later, use the following commands which will take you to the bash prompt. 

*"sudo docker start sid && sudo docker attach sid"* 

**<u>Now , we need tools to work with</u>** **:-**

Install packaging tools inside the container
`# apt-get install dh-make` OR
`# apt-get install gem2deb` OR
`# apt-get install npm2deb` as required. 

`# apt-get install git-buildpackage sbuild dh-buildinfo quilt linitan` these packages are must to be installed.

For Nodejs modules, use `npm2deb`; for ruby gems, use `gem2deb`; for go packages, use `dh-make-golang`. If there is no tool specific to a language, `dh-make` can be used as a generic tool for any language.

### Still having an issue with setting up things ?

Join our real-time chat group using one of the 4 options and say hi. If you have difficulty with pre-requisites please ask here or on chat rooms.

1. Matrix: #debian-browserify:diasp.in
2. IRC: #debian-browserify on irc.oftc.net
3. Telegram: <https://t.me/debian_browserify>

If you are new to these technologies, I suggest you try matrix, see [https://matrix.org](https://matrix.org/) for details. [https://riot.im](https://riot.im/) has a list of apps you can use.

<http://webchat.oftc.net/?channels=debian-browserify&uio=MT11bmRlZmluZWQb1> to join the IRC room



***Okay to start with packaging, start with nodejs packages and understand the workflow and then you can shift with other language modules and then to higher multi language modules.*** - [Manas Kashyap](http://wiki.debian.org/ManasKashyap)



------

**<u>Lets , do it then ?</u>** 

Install a text editor (I like vim , for installation run sudo apt install vim ) and
update .bashrc of docker container with the following environment variables

```
export DEBEMAIL=your@email.domain
export DEBFULLNAME='Your Name'
alias lintian='lintian -iIEcv --pedantic --color auto'
alias git-import-dsc='git-import-dsc --author-is-committer --pristine-tar'
alias clean='fakeroot debian/rules clean'
export LANG=C.UTF-8
export LC_ALL=C.UTF-8
```

Command ls -la shows the hidden .bashrc file , to edit with vim do vim .bashrc and
add the above and the very end , update your current environment by running

```
source ~/.bashrc
```

**vim tips**
**i – insert mode to enter text ,**
**:wq – write and exit ,:q!– exit without saving**

*Setup username and email address for git*

*git config --global user.name "Your name"*
*git config --global user.email email@domain.tld*

#### workflow (we are taking example of node module [qw](https://www.npmjs.com/package/qw))

1) Check depedency use npm2deb depends

```
$ npm2deb depends -b -r qw
```

*Module qw has no dependencies.*
*Here qw has no depedencies*

2) Search for existing work using npm2deb search

```
$ npm2deb search qw
```

*Looking for similiar package:*

*None*

*Looking for existing repositories:*

*None*

*Looking for wnpp bugs:*

*None*

Output shows no existing work and one can start work on it

3) Preview more info using npm2deb view

```
$ npm2deb view qw
```

*Version: 1.0.1*

*Description: None*

*Homepage: https://github.com/iarna/node-qw#readme*

*License: ISC*

*Debian: None (None)*

*License is automatically recognize as BSD, if this does not happen, please*
*pass*
*--upstream-license option during creation to set a correct license.*

4) Automate debian package creation using npm2deb create .

 npm2deb fails to do it completely ,our job is to fix those error which is shown by the prefix FIX_ME

```
$ npm2deb create qw
```

'You may want fix first these issues:

qw/node-qw-1.0.1/debian/control:Description: FIX_ME write the Debian package description

qw/node-qw_itp.mail:Subject: ITP: node-qw -- FIX_ME write the Debian package description 

ls command shows npm2deb created a dir called qw , 

change directory to qw, It has the following contents

$ ls

qw

$ cd qw/

$ ls

node-qw
node-qw-1.0.1.tar.gz
node-qw_1.0.1-1.dsc
qw_1.0.1- 1_amd64.buildinfo node-qw_1.0.1.orig.tar.gz
node-qw-1.0.1 node-qw_1.0.1-1.debian.tar.xz node-qw_1.0.1-1_all.deb
node- qw_1.0.1-1_amd64.changes node-qw_itp.mail

<u>node-qw is a temporary folder created by npm2deb its better to delete it to avoid confusion later</u>

$ rm -rf node-qw

5) Import package to git

```
$ gbp import-dsc --pristine-tar node-qw_1.0.1-1.dsc
```

now we get a new dir called node-qw which is git tracked , cd into the new created dir node-qw and see git branch and git tag

$ git branch

* master

  pristine-tar

  upstream

$ git tag

debian/1.0.1-1

upstream/1.0.1

Delete the debian tag by running git tag -d debian/1.0.1-1



6) File ITPFiling itp is a method by which we take ownership of the module by mailing to submit@bugs.debian.org , in return we get a bug number , a sample mailing templete is created by npm2deb for our use

$ cat node-qw_itp.mail (to view the template )

 update the template by fixing the error with the prefix FIX_ME: 

sample itp is attached below https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=881423



7) Make package lintian clean

  /qw/node-qw$ ls

LICENSE README.md debian package.json qw.js test

```
/qw/node-qw$ dpkg-buildpackage && lintian ../node-qw_1.0.1-1.dsc
```

  lintian points out the errors we need to fix

  I: node-qw source: file-contains-fixme-placeholder debian/control:20  FIX_ME

  W: node-qw source: out-of-date-standards-version 4.1.1 (current is 4.2.1) 

  places where errors where fixed regarding this module

  debian/control

  debian/copyright

  debian/changelog

 make sure to commit your changes as they are fixed by using the git command

  git commit -m “ commnet description” path/of/fixed/file

  for reference -https://salsa.debianorg/js-team/node-qw.git/tree/



8) Enable tests if present any , find the test command from package.json

```
  $ cat package.json

  {
  "name": "qw",
  "version": "1.0.1",
  "description": "Quoted word literals!",
  "main": "qw.js",
  "scripts": {
  "test": "tap test"
  },
  "keywords": [],
  "author": "Rebecca Turner <me@re-becca.org> (http://re-
  becca.org/)",
  "license": "ISC",
  "devDependencies": {
  "tap": "^8.0.0"
  },
```

  add the test command under override_dh_auto_test from debian/rules

```
override_dh_auto_test:

 tap -J test/*.js
```

add the test framework (mocha, node-tape etc) as a build dependency in

debian/control

Build-Depends:

debhelper (>= 9)

, dh-buildinfo, nodejs

, node-tap

Standards-Version: 4.1.2

add a Test-Command section to debian/tests/control

```
$ cat tests/control
  Tests: require
  Depends: node-qw
  Test-Command: tap -J test/*.js
  Depends: @, node-tap
```

  reference link ;-https://salsa.debian.org/js-team/node-qw.git/tree/debian

run dpkg-buildpackage and check if the tests ran sucessfully and change debian/changelog to release by running dch -r.



9) upload to https://salsa.debian.org ( as the team maintains it)

  create a new project as per your node-module name

  git remote add origin git@salsa.debian.org:username/node-module-name.git (eg link)

  git push -u origin --all –follow-tags

  this is a temporary repo once you mature ,will get official repo access



10) Clean build with sbuild

  

```
$ sudo sbuild-adduser $LOGNAME
```

  ... *logout* and *re-login* or use `newgrp sbuild` in your current shell$ sudo sbuild-createchroot --include=eatmydata,ccache,gnupg unstable

/srv/chroot/unstable-amd64-sbuild http://deb.debian.org/debian

```
  $ sudo sbuild -A -d unstable ../node-qw_1.0.1-1.dsc
```



  11)Install deb package

```
  apt-get install autopkgtest
  dpkg -i ../node-qw_1.0.1-1_all.deb
```



------



## Why i do Debian Packaging and how it helped me ?

I do Debian Packaging, because it helps me to learn about languages , Linux systems etc , and also its one of the way to contribute to a bigger Open Source project. Plus, it makes you feel good when something you made is used by 100 of other people. 

How it helped me ? yeah , it helped me to make a name in open source field. 
Helped me to be part of communities and gave me opportunities to get mentored by some known guys , Like [Praveen](https://qa.debian.org/developer.php?login=Praveen@debian.org) , [Raju](https://qa.debian.org/developer.php?login=rajudev@disroot.org) , etc. 

Want to see my work , here is the [qa page](https://qa.debian.org/developer.php?login=manaskashyaptech@gmail.com) and here is the [salsa page](https://salsa.debian.org/Manas-kashyap-guest) where Debian works (*oh , i forgot , Debian has its own has self hosted git instance like [github](http://github.com) , [salsa](http://salsa.debian.org) is there , isn't it cool* :-p )



------

**This Blog is just a peek into packaging and how i started and where to get help , etc .**

**Hope it helps you** **!!**




[1]: https://www.debian.org