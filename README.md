# NodeJS Droplet API

Hello World! First ever Digital Ocean Droplet using NodeJS Express.

Commands:
```
npm i express
npm i nodemon -D
```
URL: http://139.59.9.117/

___

## Getting started after deploying NodeJS
After you created a Droplet, you can navigate to its public IPv4 address to see the sample application live.

To learn how to make modifications or get your code onto your Droplet, here are the steps. You can also follow the sample application for the instructions.

### Step 1: Access to your Droplet

Open a terminal on your computer to access your Droplet as the root user using the following command:

```
ssh root@139.59.9.117
```
You will then be prompted to enter a password. If you created your Droplet with a root user password, enter this in the terminal. If you created your Droplet with an ssh key, enter the passphrase associated with your key.

### Step 2: Make modifications to the sample application

Modify this script at /var/www/html/hello.js. You can then see the results live by using pm2, a process manager that schedules your code to run at boot time. The application runs as the nodejs user, so changes to pm2 need to be run as the nodejs user.

```
sudo -u nodejs pm2 restart hello
```

### Step 3: Get your code on here

Clone your NodeJS code onto the droplet, anywhere you like. Note: If you're not using a source control, you can directly upload the files to your droplet using SFTP.

Cd into the directory where your NodeJS code lives, and install any dependencies. (For example, if you have a package.json file, run npm install.)

Launch your app

```
sudo -u nodejs pm2 start <your-file>
```

Map the port your app run on to an HTTP URL
```
nano /etc/nginx/sites-available/default
```

Edit the existing entry that exposes the "hello" app at port 3000 to the world so that it points to your app’s port instead.

Enable your new nginx config.
```
sudo systemctl restart nginx
```
Call to schedule your code to run at launch.
```
sudo -u nodejs pm2 save
```

Repeat these steps for any other NodeJS apps that need to run concurrently -- schedule them to run at boot time on whatever internal port you like using PM2, then map that port to an HTTP/HTTPS URL in the nginx config. Build out the URL directory structure you need by mapping applications to URL paths; that's the reverse proxy method in a nutshell!

Now you can delete the sample app we installed on your Droplet. Stop running the initial script (that you’re viewing now)
```
sudo -u nodejs pm2 delete hello
```

Stop it from running on Droplet boot.

sudo -u nodejs pm2 save
Step 4: Get production-ready

There's a lot you'll want to do to make sure you're production-ready. Here are the popular things that people will do.

Non-root user: Set up a non-root user for day-to-day use

Firewall: Review your firewall settings by calling sudo ufw status, and make any changes you need. By default, only SSH/SFTP (port 22), HTTP (port 80), and HTTPS (port 443) are open. You can also disable this firewall by calling sudo ufw disable and use a DigitalOcean cloud firewall instead, if you like (they're free).

Domain: Register a custom domain

Storage: You can mount a volume (up to 16TB) to this server to expand the filesystem, provision a database cluster (that runs MySQL, Redis, or PostgreSQL), or use a Space, which is an S3-compatible bucket for storing objects.