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
    * Copy **ALL** of the content
* Add new SSH Key to your Github account under the Account Settings.
    * Under Account Settings click the "Add Key" button
    * Enter a server title
    * Paste the RSA file content into the text area marked "Key"
    * Add Key

###Apache Install and Cofig
1.Install Apache 2
* `sudo apt-get install apache2`

2.Configure ServerName
* Restart Server
    * `sudo service apache2 restart`
* It will fail to restart
*Configure ServerName
    * `sudo pico /etc/apache2/conf.d/security`
        * Add
            * ServerName localhost
    * `sudo service apache2 restart`
    * Should report a successful restart

3.Restrict Access
* `sudo pico /etc/apache2/conf.d/security`
    * Uncomment <Directory/>
    * Add
        * `Options FollowSymLinks`
    * `sudo service apache2 restart`

4.Change Permissions to Allow Access
* `sudo chown -R UserName /var/www`


