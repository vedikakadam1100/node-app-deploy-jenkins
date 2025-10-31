#  Node.js Deployment on Jenkins Using Freestyle Project

Automating your Node.js application deployment with Jenkins helps you deliver updates quickly, reliably, and consistently. This tutorial will walk you through setting up Jenkins freestyle projects to pull your code from GitHub, install dependencies, and deploy your Node.js app using pm2.

## Prerequisites
Before you start, make sure you have the following in place:
Before you start, make sure you have the following in place:

 * **A Linux server** (Ubuntu/Debian recommended) with Jenkins installed and running. Jenkins runs as a dedicated user (jenkins).

* Jenkins user has **sudo privileges** so it can install software and manage processes.
* **Git plugin** installed in Jenkins.

* Node.js application source code hosted on a Git repository (e.g., GitHub).

* Open Jenkins web UI port (default **8080**) accessible via your security groups.

* Open your Node.js application port (e.g., **3000**) in security group to allow external access.

## Setting Up Jenkins Freestyle Project
We will create four Jenkins freestyle jobs to automate the deployment pipeline:

* Job-1: **01-setup-env**: install node, npm and pm2(globally)

* job-2: **02-node-pull**: Pull latest source code from Git.

* job-3: **03-install-dependency**: Install Node.js dependencies (npm install).

* job-4: **04-deploy-node**: Deploy/start the Node.js application using PM2.

## Job-1: setup environment
* Click New Item.
* Enter name: `setup-env`.
* Choose Freestyle project, click OK.
![](./img/Screenshot%202025-10-30%20172042.png)

### Bulid Steps > Execute shell:
```
sudo apt install nodejs -y
sudo apt install npm -y
sudo npm install -g pm2
```

![](./img/Screenshot%202025-10-30%20172136.png)

### Add Post-bulid Action > Bulid other project

* Enter downstream job's name:     `02-node-pull`

* Then click save
## Job-2: Pull repo from github
 * Click New Item.

 * Enter name: `02node-pull`.

 * Choose Freestyle project, click OK.

 * Scroll to Source Code Management, select Git.

 * Enter your Git repository URL, e.g.: https://github.com/iamtruptimane/node-js-app-CICD.git

 * Branch: `main`(or whatever your default is)

 ![](./img/Screenshot%202025-10-30%20172250.png)

 ### Add Post-bulid Action > Bulid other project 
 * Enter downstream job's name: `03-install-dependency`
 * Then click save

 ## Job-3: Install Dependencies

  * Click New Item.

 * Enter name: `03-install dependency`.

 * Choose Freestyle project, click OK.

 * Bulid Steps > Execute shell:
   ```
    cd /var/lib/jenkins/workspace/ node-pull-repo
   sudo npm install
   ```
  ![](./img/Screenshot%202025-10-30%20172330.png)

  ### Add Post-bulid Action > Bulid other project

   * Enter downstream job's name:`03-install-dependency`

   * Then click save

 ## Job-4: Install Dependencies

 * Click New Item.

 * Enter name: `03-install-dependency`.

 * Choose Freestyle project, click OK.

 * Bulid Steps > Execute shell:
  ```
 cd /var/lib/jenkins/workspace/02-node-pull
  ```
 pm2 start app.js --name node-app || pm2 restart node-app

 ![](./img/Screenshot%202025-10-30%20172401.png)

  ### Add Post-bulid Action > Bulid other project

 * Enter downstream job's name: `04-deploy-node`
* Then click save

  ### Running the Pipeline
  1 Go to Jenkins dashboard.

  2 Click on **01-setup-env**.

  3 Click **Build Now**.

  4 This will trigger the downstream jobs in order:

**01-setup-env** (install node, npm and pm2)
then **node-pull** (pull code),
then **node-install-dependency**(install dependencies),
then **node-deploy-node**(deploy app).

![](./img/Screenshot%202025-10-30%20172531.png)

## Access Application
open browser and go to
`http://<Public-IP>:3000` 


![](./img/Screenshot%202025-10-30%20172000.png)

## Conclusion
By setting up Node.js deployment on Jenkins using freestyle projects, you’ve built a simple yet effective Freestyle pipeline that automates pulling code from a repository, installing dependencies, and deploying the application with pm2.

This approach not only saves time but also reduces human errors, ensuring your application is always up-to-date and running smoothly. While freestyle projects are great for getting started, you can later upgrade to Jenkins Pipeline as Code for more flexibility, scalability, and better maintainability.

With the right automation in place, you can focus more on building features and less on managing deployments — turning ideas into running applications faster than ever!




