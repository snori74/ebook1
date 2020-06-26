# Day 0 - Creating Your Own Server - with a $5 Digital Ocean plan

## INTRO
First, you need a server. You can't really learn about administering a remote Linux server without having a one of your own - so today we're going to buy one!

Through the magic of Linux and virtualisation, it's now possible to get a small Internet server setup almost instantly - and at very low cost. Technically, what you'll be doing is creating and renting a VPS  ("Virtual Private Server"). In a datacentre somewhere a single physical server running Linux will be split into a dozen or more Virtual servers using the KVM (Kernel-based Virtual Machine) feature that's been part of Linux since early 2007. There are many hundreds of hosting companies offering low cost VPS deals - and sites like http://lowendbox.com/ that compare them.

As well as a hosting provider, we also need to choose which "flavour" of Linux to install on our server. If you're new to Linux then the range of "distributions" available can be confusing - but the latest LTS ("Long Term Support") version of Ubuntu Server is a popular choice, and what you'll need for this course. 
 
These instruction will walk you through using Digital Ocean (http://digitalocean.com) as your VPS hosting provider. They are rated highly, with a very simple and slick interface - and low cost of $5 (USD) per month for the minimal server that you'll be creating. (Of course, if you have a strong reason to use another provider, then by all means do so, but be sure to choose Ubuntu Server 18.04)

## Signing up with Digital Ocean
Signup is immediate - just provide your email address and a password of your choosing and you're in!

* Choose "Manage, Droplets" from the left hand sidebar. (a "droplet" is Digital Ocean's cute name for a server!)
* Select the image "Ubuntu 18.04"
* For plan, choose "Starter"
* You'll be prompted to start a $40/m plan, but select "Show all plans', and select the $5/mo one - that's fine for this course.
* You don't need to add any block storage
* Select whichever region you wish. 
* Authentication - choose "One time password"
* Choose a hostname, because the default ones are pretty ugly

## Changing the root password
You should have received a password for the "root" user in your email. Select your droplet and "Access" from the left hand sidebar and you should be able to login to the console using this. Use the login name "root", and note that the password won't show as you type or paste it.

You'll be immediately prompted to change your password.

(you're first asked for the *current* password, then asked to provide a new one of your own and then to confirm this)

Be sure to create a long and secure password - this is important, because your server is on the open Internet and will be under immediate and sustained attack from bots attempting to "brute force" the root password.

## Creating a working admin account

We want to follow the Best Practice of not logging as "root" remotely, so we'll create an ordinary user account, but one with the power to "become root" as necessary, like this:

    sudo adduser snori74
    sudo usermod -a -G adm snori74
    sudo usermod -a -G sudo snori74

(Of course, replace 'snori74' with your name!) 

*This* will be the account that you use to login and work with your server. It has been added to the 'adm' and 'sudo' groups, which on an Ubuntu system gives it access to read various logs and to "become root" as required via the _sudo_ command.

## You are now a sysadmin

Logout as *root*, by typing logout or *exit*, then login as your new sysadmin user, and confirm that you can do administrative tasks by typing:

    sudo apt update

(you'll be asked to confirm your password)

Then:

    sudo apt upgrade

Don't worry too much about the output and messages from these commands, but it should be clear whether they succeeded or not. These commands are how you force the installation of updates on an Ubuntu Linux system, and only an administrator can do them.

## We can now safely disable login as the *root* user

With our new working user able to perform all sysadmin tasks, there is no reason for us to login user *root*. Our server is exposed to all of the internet, and we can expect continuous attempts to login from malicious bots - most of which will be attempting to login as *root*. While we did set a very secure password just before, it would be nice to know that remote login as *root* is actually *impossible* - and it's possible to do that with this command:

    sudo usermod -p "!" root
    
This disables direct login access, while still allowing approved logged in users to "become root' as necessary - and is the normal default configuration of an Ubuntu system. (Digital Ocean's choice to enable "root" in their image is non-standard).  


To logout, type _logout_ or _exit_.

Your server is now all set up and ready for the course!

## Remote access via SSH

You should see an "IPv4" entry for your server, this is its unique Internet IP address, and is how you'll connect to it via SSH (the Secure Shell protocol) - something we'll be covering in the first lesson. 

Note that:
* This server is now running, and completely exposed to the whole of the Internet
* You alone are responsible for managing it
* You have just installed the latest updates, so it should be secure for now
