---
layout:     post
title:      Ansible & Ansible Roles
date:       2020-05-31 12:00:00
author:     Manas kashyap
summary:    Ansible and its roles
categories: jekyll
thumbnail:  ansible
tags:
 - linux
 - Open source
 - Ansible
 - Devops
 - Automation
---

***Today we are going to talk about an automation tool that is really common and this blog is just an overview of what i learned and how i see through it .*** 

As we all know DevOps Lifecycle is really a big lifecycle with no end and it involved numerous tools for different work in production. 
Automation is crucial these days, with IT environments that are too complex and often need to scale too quickly for system administrators and developers to keep up if they had to do everything manually. Automation simplifies complex tasks, not just making developers’ jobs more manageable but allowing them to focus attention on other tasks that add value to an organization. In other words, it frees up time and increases efficiency.

![DevOps Tutorial — Why & What is DevOps? - Edureka - Medium](https://miro.medium.com/max/1388/1*F0Sqmqz3ubCzxYAAup2oLQ.png)

So here you can see Ansible is a tool for Deployment phase , (Above is an infinity DevOps Cycle). 

## So , What is Ansible ?

Ansible is an open-source automation tool, or platform, used for IT tasks such as configuration management, application deployment, intraservice orchestration, and provisioning.

## Advantages of Ansible

- **Free**: Ansible is an open-source tool.
- **Very simple to set up and use**: No special coding skills are necessary to use Ansible’s playbooks (more on playbooks later).
- **Powerful**: Ansible lets you model even highly complex IT workflows. 
- **Flexible**: You can orchestrate the entire application environment no matter where it’s deployed. You can also customize it based on your needs.
- **Agentless**: You don’t need to install any other software or firewall ports on the client systems you want to automate. You also don’t have to set up a separate management structure.
- **Efficient**: Because you don’t need to install any extra software, there’s more room for application resources on your server.


## Ansible Architecture

Now let’s talk a bit about the pieces that make up the Ansible environment.

### Modules

Modules are like small programs that Ansible pushes out from a control machine to all the nodes or remote hosts. The modules are executed using playbooks , and they control things such as services, packages, and files. 
Ansible provides more than 450 modules for everyday tasks.

### Plugins

As you probably already know from many other tools and platforms, plugins are extra pieces of code that augment functionality. Ansible comes with a number of its plugins, but you can write your own as well. Action, cache, and callback plugins are three examples.

### Inventories

All the machines you’re using with Ansible (the control machine plus nodes) are listed in a single simple file, along with their IP addresses, databases, servers and so on. Once you register the inventory, you can assign variables to any of the hosts using a simple text file. 

### Playbooks

Ansible playbooks are like instruction manuals for tasks. They are simple files written in YAML, which stands for YAML Ain’t Markup Language, a human-readable data serialization language. Playbooks are really at the heart of what makes Ansible so popular is because they describe the tasks to be done quickly and without the need for the user to know or remember any particular syntax. 

Each playbook is composed of one or multiple plays, and the goal of a play is to map a group of hosts to well-defined roles, represented by tasks.

### APIs

Various APIs (application programming interfaces) are available so you can extend Ansible’s connection types (meaning more than just SSH for transport), callbacks and more.

# What is Ansible Roles ?

Roles is the primary mechanism for breaking a playbook into multiple files. This simplifies writing **complex playbooks**, and it makes them easier to reuse. The breaking of playbook allows you to logically break the playbook into reusable components.

Each role is basically limited to a particular functionality or desired output, with all the necessary steps to provide that result either within that role itself or in other roles listed as dependencies.

------

**<u>So that was a hell lot of theoretical part , now lets get to practical and i would like you to play with the playbook and explore the roles :-)</u>**

## How to create Roles ?

The built-in *ansible-galaxy* command has a sub command that will create our role skeleton for us.

Simply use `ansible-galaxy init ` to create a new role in your present working directory. You will see here that several directories and files are created within the new role:

![Ansible Roles example](https://linuxacademy.com/site-content/uploads/2019/05/roles11.png)

### Directory Structure

These many number of files and directories may appear to be difficult to work with, but they are fairly easy to understand. Above all, we always have the freedom to write our tasks and variable into other files but we must **include** directives into the directory’s *main.yml* file. 

Let us look into the use of these different directories in our role.

#### Defaults:

The *defaults* directory is for defining the variable defaults. The variables in *default* have the lowest priority thus becoming easy to override. If definition of a variable is nowhere else, the variable in *defaults/main.yml* will be used.

#### Files:

We use the *files* directory to add files that are needed by machine, without modification. Mostly, we use copy task for referencing files in the *files* directory.

#### Handlers:

The *handlers* directory is used for storing Ansible handlers.

A Handler is exactly the same as a Task, but it will run when called by another Task. A Handler will take an action when called by an event it listens for.
This is useful for secondary actions that might be required after running a Task, such as starting a new service after installation or reloading a service after a configuration change.

#### Meta:

We use the *meta* directory to store authorship information which is useful if we choose to publish our role on *galaxy.ansible.com*. The metadata of an Ansible role consists of author, supported platforms, and dependencies.

#### Tasks:

The *task* directory is where we write most of our roles which includes all the tasks our role will perform. We write each series of tasks in a separate file and include them into the *main.yml* file in the *tasks* directory.

#### Templates:

We use the *template* directory to add templates which can create the required file by replacing the variables . Only difference between *template* and *files* directories is that the *template* directory contains files which can be modified basically the templates. [Jinja2](https://jinja.palletsprojects.com/en/master/) language to used to create these alteration. Configuration files are basically the template files

#### Tests:

This directory contains a sample inventory and a *test.yml* file. This may contain the tests that we can do before hand in this automation process .

#### Vars:

This is where we create variable files that define necessary variables for our role.

## **Hope this helps you out and Thank you for reading this blog !!!!** 