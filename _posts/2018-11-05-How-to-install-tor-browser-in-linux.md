---
layout:     post
title:      How to install Tor-Browser in Linux
date:       2018-11-05 20:00:00
author:     Manas kashyap
summary:    A quick guide on how to install TOR browser in Linux
categories: jekyll
thumbnail:  tor
tags:
 - Proxy
 - Browser
 - Tor
 - Installation
 - linux
---
In this Blog, I will be sharing a quick guide on how to install Tor-browser in your system.

## What is Tor? 

The Tor software protects you by bouncing your communications around a distributed network of relays run by volunteers all around the world: it prevents somebody watching your Internet connection from learning what sites you visit, it prevents the sites you visit from learning your physical location, and it lets you access sites which are blocked.

Tor Browser lets you use Tor on Microsoft Windows, Apple MacOS, or GNU/Linux without needing to install any software. It can run off a USB flash drive, comes with a pre-configured web browser to protect your anonymity, and is self-contained (portable).

To know more read the official [Documentation][1] here.

*That's too much of info, get me to the installation part*

## How to install Tor. 

1) "For Kali Linux and Debian repository"
#### sudo apt-get install tor

2) Download and Running TOR Bundle
For this, you need a little grip of some basic Linux command like wget, cd and changing the permission of files.
So, let's start
I) Let's get the TOR Bundle (latest version as on 05-11-2018), we will be using "wget" tool.

So, Let's get to the terminal using ctrl+alt+t (for many Desktop environment, Defaultly setup).
Lets get to the directory where you want to install it, lets suppose Documents is the place , so the command will be *cd Documents*

Now we will download the TOR bundle using wget or you can do it manually from the [site][2] (but doing manually will Download it to the Downoads folder) , but from terminal we will use *"wget https://www.torproject.org/dist/torbrowser/8.0.3/tor-browser-linux64-8.0.3_en-US.tar.xz"* (latest bundle as per 05/11/18) .

II) Now we will extract it from the zipped filee that we just downloaded.
For that we will use *"tar -xvzf https://www.torproject.org/dist/torbrowser/8.0.3/tor-browser-linux64-8.0.3_en-US.tar.xz"* 

III) Now we can see the files will be extracted to a new folder named tor-browser(tab completion completes it)
We will go to that folder using *"cd tor-browser(tab completion)"*

IV) Now by typing *"ls -la"* , you can see a number of files in which one is "start-tor-browser.desktop"
so , to run and setup things , we will use command *"chmod +x start-tor-browser.desktop && ./start-tor-browser.desktop "*

chmod changes the permission of the file of start-tor-browser.desktop file.

##### Hope it helps you all. 


[1]: https://www.torproject.org/docs/documentation.html.en
[2]: https://www.torproject.org/download/download-easy.html.en

