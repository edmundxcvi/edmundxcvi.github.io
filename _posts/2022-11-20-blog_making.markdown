---
layout: post
title:  "Setting up a GitHub Blog"
date:   2022-11-20 22:37:00 +0000
categories: SSH Git GitHub Jekyll Site Blog
---

I've learned a lot about programming from the personal blogs of helpful developers, and I've always wanted to put something back into that ecosystem. That's why I started this blog, and this post is about how I set it up using GitHub and Jekyll. The actual steps to make a blog using Jekyll are almost comically simple, and the majority of this post is about setting up how to access  GitHub using SSH!

Making a Repository
-------------------

What is Git?
============

Version control software is the cornerstone of modern development, enabling convenient management and tracking of changes to projects. The most popular version control software is Git, in which the entire project and all previous versions are stored in a main database (known as the 'repository' or 'repo'). When users 'check out' a copy of the repo they don't just get the latest version, but also all previous changes. They can then make changes and add them to the central repository by staging and committing them. Once the changes are pushed to the central repository they become part of the indelible record of the project, and are available to all other developers next time they update their copy of the repository.

Configuring Git
===============

While you can use Git in Windows directly, it's not a fun process. Luckily, because we've previously set up [WSL](https://edmundxcvi.github.io/wsl/code/2022/11/10/system-config.html), we don't have to go through that headache. Ubuntu comes with Git preinstalled, and so you can clone public repos automatically. However, to edit Git repositories we need to configure Git so that it has some personal information about us. To add your name run `git config --global user.name name` in a Terminal window, replacing `name` with your real name, i.e. `"FirstName LastName"`. Note that you need to use quotation marks if your name has a space in it. The other essential piece of information is your email address, which you can add using `git config --global user.email email`, where `email` should be replaced by your email address.

What is GitHub?
===============

GitHub is a software hosting service provided by Microsoft which facilitates software development using Git. It acts as the database outlined above, storing the central copy of the repository from which all developers work. GitHub is a mainstay in the open-source software community, providing free public software hosting so long as the code of the project is publically accessible. As a neat little extra, they'll also host a website on your behalf, also for free. It's not the most complete web-hosting option, but for the price it simply can't be beaten!

Making the Repository
=====================

If you don't already have a GitHub account, go ahead and make one. Once you've logged in, make a new repo by clicking the '+' button in the top right of the website. Name your repository `username.github.io` (where username is your GitHub user name) and make sure the repo is public, Then click 'Create repository'. Congrats, you now have a website! (You can see in the screenshot below that I can't make a repository with the right name because it already exists, but you shouldn't have this problem.)

![MakeReop](/assets/2022-11-20/MakeRepo.png)

Cloning the Repository
======================

To clone the repository to your local machine, you'll need to get the appropriate link. Copy this to your clipboard by clicking the green 'Code' button. By default you'll be offered a HTTPS link, which you can copy to your clipboard by clicking the icon to the right of the link. Then open a Terminal window and navigate to the folder you want to copy the repo to (I use `~/projects`). Now clone the repo by running `git clone URL`, where `URL` is replaced by the link you copied earlier. This will create a folder in your current directory with the name of the repo, inside which will be the current files of your repository.


Setting up SSH
--------------

The previous step was a bit of a red herring. You can clone your repo without authentication, but if you make any changes and want to commit them back to the main copy you will need to authenticate the changes. If you used a HTTPS link to clone your repository, you will need to configure a [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token). I'm more familiar with SSH (Secure SHell), and so that's my preferred method of authenitcation.

What is SSH?
============

SSH is a protocol by which access can be granted to a remote system (like GitHub). In general, this can be performed using a username and password combination, however it is generally preferred to use an SSH key. Keys consist of two parts: a 'public' key which is uploaded to the remote server, and a 'private' key which individual users have on their local machines. SSH authentication in GitHub necessitates the use of keys, and so to access GitHub via SSH the first step is, of course, to make a pair of keys.

Making SSH Keys
===============

SSH information is stored in the `~/.ssh` directory, so navigate there (`cd ~/.ssh`). If you get an error saying that the directory does not exist, then make it by running `mkdir ~/.ssh`. Making the keys is very simple - just run `ssh-keygen -t ed25519 -C email`, where `email` is replaced by the email address associated with your GitHub account. 'ed25519' is an encryption standard used to secure your connection. Follow the prompts to save inside the .ssh folder, and secure your key with a passphrase. Your passphrase doesn't need to match the passwords for any of your accounts, but you will need to remember it! If you run `ls` you should now see two keys: `id_ed25519` and `id_ed25519.pub`. You can rename these to something more memorable if you want, but make sure they match. 

Add SSH Key to GitHub
=====================

To upload the public key to GitHub, go to your GitHub account settings on the website and navigate to the 'SSH and GPG Keys' section. From here click 'New SSH Key'. You can now give this key an informal name so you can recognise it later (ideally it should be something to do with the comptuer/account you are developing on). Now copy the contents of your public key file, paste it into the 'Key' box, and click 'Add SSH Key'. (To get the contents of the key you can run `cat id_ed25519.pub` and copy the result from Terminal.)

Configure SSH
=============

Now that we have a private key on our computer and a public key on GitHub we need to configure the connection between the two computers. To do this, navigate back to `~/.ssh` in Terminal and run `nano config`. You should enter a Terminal text editor called Nano. Now type:

{% highlight ruby %}
Host github.com
        User git
        Hostname github.com
        PreferredAuthentications publickey
        IdentityFile ~/.ssh/id_ed25519
        AddKeysToAgent yes
{% endhighlight %}

and press CTRL-O and then return to save the file. (n.b. if you changed the name of your key earlier the IdentityFile line should match the new name of your key!) If you want, you can now test the connection using the instructions found [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection).


Making a Blog
-------------

Cloning the Repository (Again)
==============================

To use SSH to edit the repository, we will need to re-clone it using the SSH link. First, delete the previous clone (`rm -rf reponame`, where `reponame` is the name of your repository directory). Next, copy the SSH link to your repository from the GitHub website, and run `git clone URL`, where URL is the SSH link.

Set up Jekyll
=============

To install Jekyll follow the instructions [here](https://jekyllrb.com/docs/installation/ubuntu/). Now change directory to be inside your repository and run `jekyll new --skip-bundle .` to create a Jekyll site. This will make a file called Gemfile, which we need to edit. Run `code Gemfile` to open it in VS Code, comment out the line starting `gem jekyll` by prepending a `#`, and uncomment the line starting `gem "github-pages". Save the changes, and then run `bundle install` from the command line to finalise the site.

Commit the Changes
==================

To 'push' our changes to the repository, first we must select the changes we want to commit. Since we want to change everything, we run `git add .`. Next, we commit the changes to our local copy using `git commit -m "MSSG"`, where `MSSG` is a short description of the change, such as `"Initial Jekyll site"`. Finally, we send our changes to the repository by running `git push`. 

Updating the Site
=================

To update the site, go back to the repository in your web browser and note the name of the branch (in the top left underneath '<> Code', it should be either 'main' or 'master'). To get the site to update automatically whenever changes are commited, go to Settings > Pages > Build and Deployment (make sure these are the repository settings not your account settings). Make sure that the source is 'Deploy from a branch' and that the Branch is the same as you noted earlier (either 'main' or 'master'. Finally, makd sure that the folder is '/ (root)'. 

Conclusions
-----------

You can now navigate to username.github.io and look at your wonderful blog! To add more pages or posts, follow the guides [here](https://jekyllrb.com/docs/posts/).