# aws-deploy-nodejs-to-nginx

open Port 80 HTTP and 22 SSH for NGINX and SSH to access ec2 instance
connect to your ec2 instance using SSH connnection
Install nginx
sudo apt update
sudo apt install nginx
check nginx is running smoothly on your public DNS or IP of your instance (make sure u are accessing your IP on HTTP not HTTPS otherwise it will through error)
Install git and clone your repository in instance or use wget (link of s3 object stored in bucket with public access) and it will copy the code into your remote machine/instance.
enter into that repo by cd dashboard (dashboard is the name of folder where code is stored)
Install all the necessary libraries of node by running command 

curl -sL https://deb.nodesource.com/setup_16.x | sudo bash -
sudo apt -y install nodejs

node -v (to check the version and node is installed or not)
npm -v (to check npm package is installed on node)
#yarn install and yarn start (to download all the necessary package required to run application and install if something else is needed to install for yarn command to work)
#Go into the folder by cd (name of folder) where your index file is present or any main file like server.js 
#type node server.js (to check if application is running smoothly and on which port like 3000 or 3001)
Install pm2 to make sure that our application runs on evrry start or restart continuously 
sudo npm install pm2@latest -g 
go into the folder
cd folder_name
sudo npm i # to build node_modules and package-lock
pm2 start server.js (server.js is main file like index file)
pm2 status to check pm2 is running your application
Configure NGINX
sudo nano /etc/nginx/sites-enabled/default
#go into root and replace root with folder path where your server.js or index is stored
#root /home/ubuntu/dashboard-admin;  (in this I have stored my file to dashboard-admin)
#make sure that you have added server.js if it is not mentioned below root 
Now at location we need to perform reverse proxy  so that it can redirect my 3001 port to 80 
COPY the below inside the location and make sure identation is correct and comment everything inside location
location / {
 
proxy_pass http://127.0.0.1:3001;
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection 'upgrade';
proxy_set_header Host $host;
proxy_cache_bypass $http_upgrade;
client_max_body_size 100M;
# It increases the size of upload to 100 MB
#
Just make sure you are using the correct port number as I am using 3001 as my application port number
save it using ctrl and O to save and exit with ctrl and Z

sudo nginx -t #to check if our configuration is done succesfully and no identation error is present

now restart nginx using "sudo systemctl restart nginx"
Now open the PUBLIC IP and DNS address to see your application running



