---
title: "Deploy Web Server"
date: "2024-01-01"
weight: 5
chapter: false
pre: "<strong>2.5. </strong>"
---

Install Node Version Manager (nvm) by entering the following command into the terminal:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
```

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.1.png?featherlight=false&width=90pc)

Run the following three lines of commands in your terminal exactly as they appear to immediately start using nvm without having to close and reopen the terminal:
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

To install Node.js using nvm, enter the following command in the terminal.

```bash
nvm install 20
```

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.2.png?featherlight=false&width=90pc)

Clone the application repository by running the following command in your terminal:

```bash
git clone https://github.com/First-Cloud-Journey/000004-EC2.git
```

Navigate to the directory for lab 000004-EC2

```bash
cd 000004-EC2
```

NPM, short for Node Package Manager, is a vital tool for managing JavaScript libraries and dependencies in Node.js applications.When you run the command npm init, it initializes a new Node.js project and creates a package.json file. This file contains metadata about the project, such as its name, version, description, and a list of dependencies.

```bash
npm init
```
You should press Enter continuously to create the default value

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.3.png?featherlight=false&width=90pc)

Install pm2 globally; PM2 is used to manage and monitor running Node.js applications. It allows applications to run in the background.

```bash
npm install -g pm2
```

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.4.png?featherlight=false&width=90pc)

Next, we redefine the script to run the application. We will use vim or nano to open the package.json file and in the scripts section under the key start, assign it the following value. This will allow our application to run in the background:

```bash
pm2 start app.js
```
Enter the following command to access the content of the package.json file:
```bash
nano package.json
```
Edit the file content as below:
![Image](/images/2-preparation/2.5-deploy-web-server/2.5.5.png?featherlight=false&width=90pc)

Continue using vim or nano to access the .env file, then enter the following content to set up the connection to the database.

```bash
DB_HOST='fcj-management-db-instance.cdysiiecu90g.ap-southeast-1.rds.amzonaws.com'
DB_NAME='awsfcjuser'
DB_USER='admin'
DB_PASS='123Vodanhphai'
```

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.6.png?featherlight=false&width=90pc)

Proceed to start the application.

```bash
npm start
```

The command **`pm2 status`** in PM2 is used to display the current status of all applications being managed by PM2. When you run this command, you'll receive an overview of each application, including details like their state (running, stopped, etc.), memory usage, CPU usage, and the number of restarts. This helps in monitoring the performance and health of your Node.js applications effectively.

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.7.png?featherlight=false&width=90pc)

Next, we need to obtain the public DNS of the instance so that we can access the application from the browser.

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.8.png?featherlight=false&width=90pc)

Our application is running stably

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.9.png?featherlight=false&width=90pc)

Next, we use the command **`pm2 startup`** to configure PM2 to automatically restart applications when the server reboots.
It will prompt you to set up a Startup Script. Please copy and paste that command and execute it.

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.10.png?featherlight=false&width=90pc)

To ensure that the running applications are saved and restarted when the server reboots, we need to run the command **`pm2 save`**. This command will save the current state of the processes to the startup list.

![Image](/images/2-preparation/2.5-deploy-web-server/2.5.11.png?featherlight=false&width=90pc)
