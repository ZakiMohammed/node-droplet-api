# Ubuntu 20.04 (LTS) x64

#### create new user
```
adduser zaki
usermod -aG sudo zaki
id zaki
sudo su - zaki
```

#### disabling root and password login
```
mkdir ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
	Paste your public key starting with and without line space ssh-rsa <your_public_key>
sudo service ssh restart
sudo nano /etc/ssh/sshd_config
	PermitRootLogin no
	PasswordAuthentication no
exi
```

#### node installation 
#### https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04
```
cd ~
curl -sL https://deb.nodesource.com/setup_16.x -o nodesource_setup.sh
nano nodesource_setup.sh
sudo bash nodesource_setup.sh
sudo apt install nodejs
node -v
```

#### git installation
```
sudo apt-get install git
git clone <your_repo_name>
git clone https://github.com/ZakiMohammed/node-droplet-api.git
```

#### pm2 installation
```
sudo npm install -g pm2
pm2 start index.js
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u zaki --hp /home/zaki
```

#### firewall
```
sudo ufw status
sudo ufw enable
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw status
```

#### nginx
```
sudo apt install nginx
sudo nano /etc/nginx/sites-available/default
	server_name yourdomain.com www.yourdomain.com;

	location / {
		proxy_pass http://localhost:5000; #whatever port your app runs on
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection 'upgrade';
		proxy_set_header Host $host;
		proxy_cache_bypass $http_upgrade;
	}
sudo nginx -t
sudo service nginx restart
```

#### ssl
`https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04`
```
sudo ufw allow 'Nginx Full'
sudo apt install certbot python3-certbot-nginx
#### in below command do not use www for subdomain
sudo certbot --nginx -d example.com -d www.example.com
sudo systemctl status certbot.timer
sudo certbot renew --dry-run
```

#### libcap2.bin installation
```
sudo apt-get install -y libcap2-bin
sudo setcap cap_net_bind_service=+ep `readlink -f \`which node\``
sudo setcap -r cap_net_bind_service=+ep `readlink -f \`which node\``
```

#### mongodb installation
`https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-20-04`

#### mongodb allow remote connection
`https://www.digitalocean.com/community/tutorials/how-to-configure-remote-access-for-mongodb-on-ubuntu-20-04`
```
sudo lsof -i | grep mongo
curl -4 icanhazip.com
	Know your own ip address
sudo ufw allow from trusted_machine_ip to any port 27017
sudo ufw status
	If status is inactive then enable ufw with following commands
	sudo ufw enable
	sudo ufw status
	sudo ufw allow ssh (Port 22)
	sudo ufw allow http (Port 80)
	sudo ufw allow https (Port 443)
sudo nano /etc/mongod.conf
	Update the file with following
	. . .
	#### network interfaces
	net:
	  port: 27017
	  bindIp: 127.0.0.1,server_ip_address

	. . .
sudo systemctl restart mongod
mongo "mongodb://username@mongo_server_ip:27017"
mongodb://zaki@143.244.138.223:27017
```

###### host multiple application on single droplet
`https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-server-blocks-virtual-hosts-on-ubuntu-16-04`