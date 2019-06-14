---
layout:     post
title:      Basics of Gitlab Installation in a Debian system 
date:       2019-06-14 20:00:00
author:     Manas kashyap
summary:    A quick guide on how to install Gitlab in a Debian based system or we can say how to create own Gitlab Instance 
categories: jekyll
thumbnail:  gitlab
tags:
 - Gitlab
 - Browser
 - VCS 
 - Debian
 - linux
 - Installation
---
In this Blog, I will be sharing a quick guide on how to install Gitlab in your Debian system.

Ever thought How [http://github.com](http://github.com/) , [https://gitlab.com](https://gitlab.com/)  works , or how can i create my own Version control system locally for just my use and testing , or ever want to create personalised git instance like  [http://github.com](http://github.com/) for your group of contributors or developers . 

Well you are at right place and at the end of the blog you can make your own git instance or you can say your own Version Control System . 

I have personally used a Debian unstable Linux container system for this whole installation process . 
i suggest to use a server based container , of Debian unstable . 

**Don't use Docker container for this installation process , there are docker pre made  [container](https://hub.docker.com/search?q=gitlab&type=image/)  available to use which can run as Gitlab server, just do "docker pull container_required"** 

## What is Gitlab or VCS ?  

The full form of VCS is Version Control System , these systems are a category of software tools that help a software team manage changes to source code over time. Version control software keeps track of every modification to the code in a special kind of database. If a mistake is made, developers can turn back the clock and compare earlier versions of the code to help fix the mistake while minimising disruption to all team members.

VCS are sometimes known as SCM (Source Code Management) tools or RCS (Revision Control System). One of the most popular VCS tools in use today is called Git . 

GitLab is a single application for the entire software development lifecycle. From project planning and source code management to CI/CD, monitoring, and security. 

To know more read the official [Documentation][1] here.

*That's too much of info, get me to the installation part*

## So how and what are we going to do in this installation ?

In my words what are we going to do is install Gitlab package of Debian in the Debian system with a little configuration and then install Gitlab runner so that the Continuous Integration and Development can be done with the help of .gitlab-ci.yml file . 

##### so the first thing is to use a Debian unstable container , i am using [lxc](https://linuxcontainers.org/) container here , there are many ways to install lxc , please visit their page to install it . 

As , when i was starting with lxc , i was facing a few issues due to lxc configuration file , how to bridge and what not to , so i am attaching my lxc configuration that i use . 

you can edit it too by going in **/etc/lxc/default.conf** 

`lxc.net.0.type = veth`

`lxc.apparmor.profile = generated`

`lxc.apparmor.allow_nesting = 1`

`lxc.net.0.link = virbr0`

`lxc.net.0.flags = up`

Once lxc is configured you can download the unstable container using 

`lxc-create -n gitlab -t debian -- -r unstable` 

and then start it with 

`lxc-start -n gitlab` 

and then just proceed into it using 

`lxc-attach -n gitlab`

**TIP :-  sometimes while searching for this configuration , you will found terms like , lxc.network.type , those are old lxc terms , if you are using lxc 2.1 or above the terms change to lxc.net.0 and else , if you have been using old lxc and update to new one and configuration fails , you don't need to panic , just do `lxc-update-config`  and its done , the old configuration will change to new configuration .**

- Once you are inside the container or the unstable server that you choose , 
  do the following `apt-get update && apt-get upgrade`
  (assuming you are having the super user permission )

**TIP :-  sometimes while doing apt-update and apt-upgrade in lxc container you will have the issue of not connecting with deb.debian.org , i faced this issue , this is because of sources.list permissions , change the permission of the file using `chmod o+r /etc/apt/sources.list`**

- Now the task is to add [contrib](https://www.debian.org/doc/debian-policy/ch-archive#s-contrib) section in the sources.list . 
  Now your sources.list will look like this if you do `cat /etc/apt/sources.list` 

  

`deb http://deb.debian.org/debian unstable main contrib non-free`

`deb-src http://deb.debian.org/debian unstable main contrib non-free`



- Now let's install the [gitlab](https://tracker.debian.org/pkg/gitlab) package . 

`apt-get update && apt-get install upgrade`

`apt-get install gitlab`


**The installation is big and will take time so be patient** 

When it will be installing it will ask for some config questions , the questions are self explanatory and will tell you what to enter and it has example of it , i run gitlab instance in my local system , so i have named the gitlab instance as gitlab.lxc (while they ask you , you can give any name like git.yourname.com , it all depends on the domain name you buy for this) and i used local host for database configuration well they give you an option of new host where you can enter the database configuration of the other database system you are using . 
And then it will run a few check and it will be done 

you can check the status of Gitlab using `systemctl status gitlab.service` 

Now as this Gitlab instance is installed on the container system , we have to update the host of the container and also the systems where you want to see this gitlab instance running to resolve the git.yourusername.com 
you can view the container ip address using ip a 
and add that ip address in /etc/hosts of the container as well as in the systems to resolve the gitlab instance name . 
`192.168.122.104 gitlab.lxc`
 Change ip address and the name of the gitlab instance you gave

*We are updating the containers host too because when we will try to setup gitlab runner , it cant resolve the instance gitlab name , so we are setting it up .* 

**Now in the system where you want to view you can view the git instance running by typing the url in the browser like "http://gitlab.lxc" depending on your gitlab instance name and whether you have https setup or not which will be asked by gitlab while configuring and installing .** 


Now you have your own Gitlab instance , just register to it , till the time you have your won domain registered and set you wont get any mail for confirmation or anything . 

But you can login to the system with the registered ID and password , and create your project , but till now we haven't setup a runner to run any Gitlab CI , CD process so , we will do it with the help of [gitlab-runner](https://tracker.debian.org/pkg/gitlab-runner) package available .

`apt-get install gitlab-ci-multi-runner` 

it will install the runner . 

<u>Now get into  admin panel of your own Gitlab instance .</u>

*Wait , how to do it and where is my admin panel ?* if these questions are coming into mind , well here is the answer , now go back to the container or server where you installed Gitlab . 

now just remember the mail id of the person you want to give the admin access to and follow the steps 

```
Granting an existing user admin access
======================================
The steps involve dropping into rails console as gitlab user for production environment and running the following commands.
    $ runuser -u gitlab -- sh -c 'cd /usr/share/gitlab && ./etc/gitlab/gitlab-debian.conf && export DB RAILS_ENV && bundle exec rails console production'
    irb(main):001:0> user = User.find_by(email: 'useraddress@domain')
    irb(main):003:0> user.admin=true
    irb(main):004:0> user.save
```

Now you have the admin access , and you can view it in your instance too when you login to gitlab instance with the email id that you gave just now giving the admin access , there will be a sign of setting on the upside and you can click and go into the admin panel which will show number of users and all . 

Now go into the runner option on the side bar and you will have the token there 

Now to register the runner ,

1. Run the following command:

   
    sudo gitlab-runner register
   

2. Enter your GitLab instance URL:

   
    Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com )
    https://gitlab.com
   

3. Enter the token you obtained to register the Runner:

   
    Please enter the gitlab-ci token for this runner
    xxx
   

4. Enter a description for the Runner, you can change this later in GitLab’s UI:

   
    Please enter the gitlab-ci description for this runner
    [hostname] my-runner
   

5. Enter the [tags associated with the Runner](https://docs.gitlab.com/ee/ci/runners/#using-tags), you can change this later in GitLab’s UI:

   
    Please enter the gitlab-ci tags for this runner (comma separated):
    my-tag,another-tag
   

6. Enter the [Runner executor](https://docs.gitlab.com/runner/executors/README.html):

    Please enter the executor: ssh, docker+machine, docker-ssh+machine, kubernetes, parallels :
    
    enter whatever you want , i personally suggest docker in this case . 




    **Tip :- In Debian unstable there is a docker package , on the name of docker.io , install it using `apt-get install docker.io` , before proceeding with the CI testing from the gitlab instance**

7. If you chose Docker as your executor, you’ll be asked for the default image to be used for projects that do not define one in `.gitlab-ci.yml`:

   
    Please enter the Docker image (eg. ruby:2.1):
    alpine:latest
   



And with this you will have your shared runner setup with which you can run CI & CD . 

**Tip :- When you want to run CI and CD please see the internet connection is strong as it will download the container and will work on it .**

And with this , now you have your own gitlab instance running with the CI & CD enabled . 


##### **I know this blog or the steps i gave may or may not worked for you , this is all based on my personal experience to work on the Debian unstable machine installation process , if you think you have a query or doubt feel free to reach me in mail or telegram or riot , hope i can help you with that or if you find any flaw in my blog , reach me out .** 

##### **Hope it helps you all.** 


[1]: https://about.gitlab.com


