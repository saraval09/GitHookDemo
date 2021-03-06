# Sara's Portfolio
My portfolio from Full Sail University

##Server Architectural Needs for Single-Tier Server:
  * Ubuntu 12.04.5 x64 LTS
  * Apache/2.2.22 (Ubuntu)
  * git version 1.7.9.5

##Deployment Plan

1.Create Server
* Create a digital ocean account
* Create Droplet
    * Name Hostname
    * Select Size
    * Select a region near you
    * Select Image (Ubuntu 12.04.5 x64)

2.SSH Into The Server
* Open Terminal
    * `ssh root@YourIpAddress` (Ip address was emailed to you)
    * Follow Prompts

3.Create Non-Root User
* add a new user
    * `adduser yourUserName`
    * Follow Prompts
    * add user to sudo
    * `adduser yourUserName sudo`

4.End SSH Session
* `exit`

5.Login as Non-Root User
* `ssh yourUserName@YourIpAddress`
* Enter Password

6.Update Package System
* `sudo apt-get update`
* Enter Password

7.Upgrade Package System
* `sudo apt-get upgrade`
* Follow Prompts

8.Update Packages for Newly Installed Version
* `sudo apt-get update`

9.Update System Level Packages
* `sudo aptitude update`
* `sudo aptitude safe-upgrade`
* Follow Prompts
* `sudo reboot`
* `ssh yourUserName@YourIpAddress`
* Enter Password

10.Install Packages
* Git
* Apache

###Git: Install and Config
1.Install Git
* `sudo apt-get install git-core`

2.Configure Git
* `git config --global user.name “NAME”`
* `git config --global user.email Your@Email.com`

3.Confirm Settings
* `git config --list`

4.Create SSH Keys for Github Access
* `ssh-keygen -t rsa -C ”YourEmail@example.com”`
    * Press enter so you **DONT** customize your name
* Enter a password (its a good idea to use your Github password to make it easier)
* Re-enter your password

5.Put the RSA key on file with github.com so this server is treated as a trusted machine.
* `less ~/.ssh/id_rsa.pub`
    * Copy **ALL** of the content from the beginning of the key (ssh) to the end of your email
* Add new SSH Key to your Github account under the Account Settings.
    * Under Account Settings click the "Add Key" button
    * Enter a server title
    * Paste the RSA file content into the text area marked "Key"
    * Add Key
6. In the terminal log into your github account
    * `ssh git@github.com`
    * Enter the password for the SSH key
    * It will say that you successfully authenticated


###Apache Install and Cofig
1.Install Apache 2
* `sudo apt-get install apache2`
* Follow Prompts
* Test Apache by putting your Digital Ocean Ip Address into the URL to see if it works

2.Configure ServerName
* Restart Server
    * `sudo service apache2 restart`
* It will fail to restart
* Configure ServerName
    * `sudo pico /etc/apache2/conf.d/security`
        * Add in one of the non commented lines
            * `ServerName localhost`
            * Press Control X to exit
            * Save Yes
            * Enter
    * `sudo service apache2 restart`
    * Should report a successful restart

3.Restrict Access
* `sudo pico /etc/apache2/conf.d/security`
    * Uncomment <Directory/>
    * Add
        * `Options FollowSymLinks`
        * Line under "Deny from all"
        * Press Control X to exit
        * Save Yes
        * Enter
    * `sudo service apache2 restart`

4.Change Permissions to Allow Access
* `cd /var/www/` which is where your index file is
* `sudo chown -R YourUserName /var/www`
* `pico index.html`
    * Add something to the title to test that it is working properly
* Go back to your browser and refresh your IP address to see the changes you did in the index file


###Configure Git Deployment
1.Create Space for Repo on Live Server
* `sudo mkdir /var/repos/`
* `sudo chown YourUserName /var/repos`
* `cd /var/repos && mkdir SiteName.git && cd SiteName.git`
* `git init --bare`
   * A bare git init means this folder will have no source files, only the version control structure of git.

2.Configure Server Post Hook
* `cd /var/repos/SiteName.git/hooks/`
* `pico post-receive`
    * Save the file with the contents below. It separates the working tree and git dir into separate locations.
        * `#!/bin/sh
           GIT_WORK_TREE=/var/www git checkout -f`
        * Press Control X to exit
        * Save Yes
        * Enter
* `chmod +x post-receive`
    * Give permissions to make the hook executable by the server.

3.Configure Local Dev Environment
* It’s important to have a centralized space for your Projects and Deployments even if you’re not deploying multiple
sites on this server.
* Open a new Terminal Window
* `mkdir ~/getHookDemo`
* `mkdir ~/getHookDemo/SiteName.com && cd ~/getHookDemo/SiteName.com`
* `git init`
* `git remote add LiveServer ssh://YourUserName@IPAddress/var/repos/SiteName.git`

###Staging
Follow the steps for "Configure Local Dev Environment" again to make another server called "Staging". It will be used before your code gets pushed to the live server to make sure everything is ready and not broken. Once everything looks great on the staging server go ahead and push it to the live server for everyone to see your beautiful site!!