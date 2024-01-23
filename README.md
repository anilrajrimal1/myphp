# Project 2: CI/CD Setup with GitHub, Jenkins, and Apache Webserver (PHP based)

## Overview

This project focuses on setting up a Continuous Integration/Continuous Deployment (CI/CD) pipeline using GitHub, Jenkins, and an Apache Webserver. The primary objectives include configuring Jenkins, Git, and Apache Webserver, integrating GitHub with Jenkins and Apache, creating CI and CD jobs, and testing the deployment of a PHP-based website.


developers --->push--> Git hub --->pull-->Jenkins -->pipeline - build --> webserver (Apache)
# Table of Contents

1. [Installing Apache Server and PHP in Webserver](#1-installing-apache-server-and-php-in-webserver)
2. [Creating New Repo in GitHub for Our PHP Project](#2-creating-new-repo-in-github-for-our-php-project)
3. [Creating a Job in Jenkins to Pull index.html from GitHub Repo and Send it to the Webserver into /var/www/html Directory](#3-creating-a-job-in-jenkins-to-pull-indexhtml-from-github-repo-and-send-it-to-the-webserver-into-varwwwhtml-directory)

## 1. Installing Apache server and php in webserver
To set up the Apache server and PHP on your webserver, follow the steps below:

- Install Apache server and PHP:
   ```bash
   # Install Apache and PHP
   sudo yum -y install httpd php

    ```
![install-httpd](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/install%20httpd%20on%20Anil.png)

- Start the Apache service:
   ```bash
   # Start the Apache service
    sudo systemctl start httpd
   
    ```

- Enable the Apache service to start on boot:
   ```bash
   # Enable Apache service for startup
    sudo systemctl enable httpd 
    sudo systemctl status httpd
    ```
![status-httpd](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/httpd%20status.png)

- Add the HTTP service to the firewall (permanent):
   ```bash
   # Add HTTP port to firewall
    sudo firewall-cmd --permanent --add-service=http
    ```

- Reload the firewall to apply changes:
   ```bash
   # Reload the firewall
    sudo firewall-cmd --reload

    ```
![firewall](https://github.com/anilrajrimal1/myhtml/blob/master/screenshots/firewall%20port.png)
These commands will install Apache server and PHP, start the Apache service, enable it for startup, and configure the firewall to allow HTTP traffic.## 2. Creating New Repo in GitHub for Our PHP Project
To create a new GitHub repository for your PHP project and initialize it locally, follow the steps below:

### Login to GitHub:
   - Create a new repository named "myphp".
   - Copy the repository URL.

![create-repo](https://github.com/anilrajrimal1/myphp/blob/master/screenshots/repo%20on%20Github.png)

- On your developer's machine, power on and create a folder for your project:
   ```bash
   # Create project workspace
   mkdir -p /root/project/myphp
   cd /root/project/myphp
   ```
![project-php](https://github.com/anilrajrimal1/myphp/blob/master/screenshots/myphp%20folder%20creation%20on%20Developer%20.png)

- Initialize a local Git repository:
   ```bash
    git init
   ```
- Add the GitHub repository as the remote origin:
   ```bash
    git remote add origin <your repo url>

    # example:- git remote add origin git@github.com:anilrajrimal1/myphp.git
   ```
- Pull the existing master branch from the GitHub repository:
   ```bash
    git pull origin master
   ```
- List the files in the project folder:
   ```bash
    ls
   ```
- Open the index.php file in a text editor and add PHP code:
   ```bash
    vi index.php

    # add your PHP code
   ```

- Add and commit changes:
   ```bash
    git add -A

    git commit -m "myphp first commit"
   ```
- Push changes to the GitHub repository:
   ```bash
    git push origin master
   ```

![git-jobs](https://github.com/anilrajrimal1/myphp/blob/master/screenshots/git%20on%20myphp%20Developer.png)

These commands create a new GitHub repository, set up a local Git repository, and push your PhP project to the GitHub repository.
## 3. Creating a Job in Jenkins to Pull index.html from GitHub Repo and Send it to the Webserver into /var/www/html Directory


To create a Jenkins job for automating the process of pulling `index.php` from a GitHub repo and sending it to the webserver, follow the steps below:

### Open the Jenkins dashboard.

### Click on "New Item" to create a new job.

### Choose "Freestyle project" and provide a name for the job.

![project-Freestyle](https://github.com/anilrajrimal1/myphp/blob/master/screenshots/creating%20myphp%20job%20.png)

### Configure the Git repository details:
   - Under "Source Code Management," select "Git."
   - Enter the repository URL and provide the necessary credentials.

![project-git-add](https://github.com/anilrajrimal1/myphp/blob/master/screenshots/source%20code%20management.png)

### Configure the Build Trigger:
   - Under "Build Triggers," select "Poll SCM."
   - Set the schedule to run the job using Cron syntax, e.g., `* * * * *` for polling every minute.

![build-Trigger](https://github.com/anilrajrimal1/myphp/blob/master/screenshots/poll%20scm.png)

### Configure the Post-Build Action:
   - Under "Post-build Actions," select "Send build artifact over SSH."
     - Choose the SSH server previously configured (webserver).
     - Set the "Source files" to `index.php`.
     - Set the "Remote directory" to `//var//www//html`.

   ```markdown
   - Jenkins Dashboard
     --> New Item (New Job)
       - Project name: <Your Job Name>
       - Type: Freestyle project
       - Source Code Management
         - Git
           - Repository URL: <Your GitHub Repo URL>
           - Credentials: <Select appropriate credentials>
       - Build Triggers
         - Poll SCM
           - Schedule: * * * * * (for polling every minute)
       - Post-build Actions
         - Send build artifact over SSH
           - SSH Server: webserver (Select server name)
           - Source files: index.php
           - Remote directory: //var//www//html

```
- Click "Save" to create the Jenkins job.

Now, Jenkins is configured to pull index.html from the GitHub repo and send it to the webserver's //var//www//html directory after each build.

#### Console Output
![console-output](https://github.com/anilrajrimal1/myphp/blob/master/screenshots/console%20output%20for%20build.png)


#### Webserver Result
![result](https://github.com/anilrajrimal1/myphp/blob/master/screenshots/on%20Anil-User.png)
