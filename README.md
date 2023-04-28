# method-to-deploy-mean-stack-on-aws



You can configure the AWS Ubuntu EC2 environment explained in the  video by running the below commands:

----------------------
  NODE & NPM
----------------------
# add nodejs 10 ppa (personal package archive) from nodesource
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -

# install nodejs and npm
sudo apt-get install -y nodejs

----------------------
  PM2
----------------------
# install pm2 with npm
sudo npm install -g pm2

# set pm2 to start automatically on system startup
sudo pm2 startup systemd

----------------------
  NGINX
----------------------
# install nginx
sudo apt-get install -y nginx

----------------------
  UFW (FIREWALL)
----------------------
# allow ssh connections through firewall
sudo ufw allow OpenSSH

# allow http & https through firewall
sudo ufw allow 'Nginx Full'

# enable firewall
sudo ufw --force enable


You can Clone the Back-End NodeJS App Explained in the video using:
sudo git clone https://github.com/vemarahub/node-bac... /usr/back-end

You can Clone the Front-End Angular App Explained in the video using:
sudo git clone https://github.com/vemarahub/Angular-... {local path}




Clone the Node.js + MongoDB API project into the /opt/back-end directory with the command sudo git clone https://github.com/cornflourblue/node-mongo-registration-login-api /opt/back-end
Navigate into the back-end directory and install all required npm packages with the command cd /opt/back-end && sudo npm install
Start the API using the PM2 process manager with command sudo pm2 start server.js


sudo mkdir /opt/front-end
Change the owner of the folder to the ubuntu user and group with the command sudo chown ubuntu:ubuntu /opt/front-end. This is to allow us to transfer the front end files in the next step.
Transfer the compiled app to the server via SSH using the PuTTY Secure Copy client with the command:
pscp -i <path-to-key-file> -r <path-to-local-dist-folder>\* ubuntu@<domain name>:/opt/front-end
E.g. pscp -i C:\Downloads\my-aws-key.ppk -r C:\Downloads\angular-8-registration-login-example\dist\* ubuntu@ec2-52-221-185-40.ap-southeast-2.compute.amazonaws.com:/opt/front-end
  
  
  
  Follow these steps to configure NGINX for the MEAN stack app.

Delete the default NGINX site config file with the command sudo rm /etc/nginx/sites-available/default
Launch the nano text editor to create an new default site config file with sudo nano /etc/nginx/sites-available/default
Paste in the following config:
server {
  charset utf-8;
  listen 80 default_server;
  server_name _;

  # angular app & front-end files
  location / {
    root /opt/front-end;
    try_files $uri /index.html;
  }

  # node api reverse proxy
  location /api/ {
    proxy_pass http://localhost:4000/;
  }
}
Save the file by pressing ctrl + x and selecting Yes to save.
Restart NGINX with the command sudo systemctl restart nginx
  
  
  
