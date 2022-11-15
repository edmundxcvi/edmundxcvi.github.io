---
layout: post
title:  "Setting up WSL"
date:   2022-11-10 19:14:00 +0000
categories: WSL Code
---

I use Windows every day. It's reliable and customisable, but most importantly it is practically unavoidable due to its effective global monopoly. However, if you want to write code then Windows can be an unnecessary obstacle, often involving long and contrived workflows to accomplish what would be a a one-click (or more likely a one-command) task in Linux. One solution is to use a Linux virtual machine inside Windows, however this can be annoying, especially if you only have one monitor. A more elegant solution is to use Windows Subsystem for Linux (WSL), Microsoft's built in way to run a Linux virtual machine without the additional resource drain associated with traditional virtual machines. This is how I set up a new user account for development, including setting up WSL.

Making a new user account
-------------------------

If you already have another account on your computer, you can make a new one by opening Settings and navigating to Accounts ► Family & other users ► Add someone else to this PC. From here you can either make a phoney windows account for your developer alter-ego, or select "I don't have this person's sign-in information" and then "Add a user without a Microsoft account" to avoid making a new account. Follow through the setup wizard to complete your new user area.

Once you're in, there's a few important things to do. The first thing I do is get rid of the useless stuff that comes preconfigured into the taskbar like search, workspaces, the weather, and whatever other pieces of bloatware Microsoft is pushing this month. Then I get rid of all the useless tiles from the start menu. All of this can be achieved by right clicking on the offending icon and choosing to hide/unpin it. The last thing to do before we get to the developer specific part is to make use of a useful feature in Microsoft Edge:

![The only use for Edge](/assets/2022-11-15/DownloadChrome.png)

Setting up WSL
--------------

Personalisation
===============

The most important thing about writing code is making sure that everybody around you knows that you can write code. The easiest way to do this is set your computer to dark mode. Do this by opening Settings and going to Personalisation ► Colours and selecting Dark from the "Choose your colour" drop-down:

![Setting dark mode](/assets/2022-11-15/DarkMode.png)

Installing WSL
==============

Installing WSL is super easy. First open a PowerShell window with admin privileges. If you don't know where PowerShell is I don't blame you - on my computer it in the "Windows PowerShell" folder in the Start menu. To open with admin privileges right click the icon and select "Run as administrator". Once its open it should look like this:

![PowerShell](/assets/2022-11-15/AdminPowerShell.png)

PowerShell is a command-line administration tool for Windows, but we're going to use it to install WSL. To run a command, type it out in PowerShell and press return. Linux comes in a lot of different forms (known as 'distros'), but if you don't care about that you can install the most common one known as Ubuntu. Theoretically, this should be as simple as running `wsl --install`, however this didn't work for me. Instead, I had to explicitly pick the form of Linux that I wanted. First I checked which distros were available by running `wsl --list --online`, and then installed Ubuntu using `wsl --install -d Ubuntu`. Follow through the prompts to make a new user (the name and password don't have to be the same as your Windows login) and you should end up in different window that looks like this:

![PowerShell](/assets/2022-11-15/UbuntuShell.png)

Congratulations! You installed WSL! Now what?

(n.b. if this didn't work, a more complete guide can be found [here](https://learn.microsoft.com/en-us/windows/wsl/install))

Quality of Life Improvements
============================

Right now you can start a WSL session any time you like by opening Ubuntu XX.XX on Windows. Personally, I prefer a more modern interface with the ability to have multiple tabs and customisable formatting. You can do this by downloading Terminal from the Microsoft Store. When you open it up for the first time it will default to PowerShell, but you can change that to your WSL shell using the settings:

![Installing Terminal](/assets/2022-11-15/TerminalInstall.png)
![Setting Default Profile](/assets/2022-11-15/TerminalDefault.png)

Now whenever you want to use WSL, just open Terminal! Moving around file systems without File Explorer can feel alien at first, so feel free to practice making a folder using `mkdir`, listing items in the current folder using `ls`, and changing to different folders using `cd`. These commands will quickly become second nature. If you ever get lost and need to go back to your home directory, just use `cd` without any arguments.

The neatest thing about WSL is that your Linux system can access your Windows files and your Windows system can access your Linux files. To access your C-drive from WSL type `cd /mnt/c`. Now type `ls` and you'll see everything in your C-drive, just like you would in File Explorer. To see your Linux files from Windows is also really easy. Just open File Explorer and type `\\wsl$\` into the Address bar (next to the up arrow). Press return and you'll see the Linux system you just installed. Your user area will be in side `\home\`, and I usually pin this for quick access in File Explorer.

![Mounting your Home Directory](/assets/2022-11-15/WSLMount.png)

Essential Maintenance
=====================

Just like it's important to keep Windows up to date, it's important to keep your WSL system up to date. Unlike in Windows, Linux will not update for you. Instead, you have to manually check and install updates. The precise way to do this varies from distro to distro, but in Ubuntu updates are managed by `apt` (the advanced package tool). To check for updates, run `sudo apt update` and to install them run `sudo apt upgrade` and confirm you want to do this when prompted.

Code Editors
------------

I've tried a couple of source code editors, but VS Code is by far my favourite. VS Code is made by Microsoft and is incredibly feature rich. It also works seamlessly with WSL, so you can edit and run code on your Linux system without even having a terminal windows open! After you [download] it and open it, you're prompted with a load of personalisation options. I think it's fine just the way it is, but play around until you get something you like. Once you're done, click the square icons on the left to get to the Extensions manager and search for WSL on the marketplace. Once the WSL extension is installed, you'll be able to a new WSL VS Code window by clicking the green arrows in the bottom left.

Conclusions
-----------

I feel like all good blog posts have conclusions, but really this is only the start. My next post will be on how I got this blog working as a github site. Stay tuned - hopefully it will go up very soon!