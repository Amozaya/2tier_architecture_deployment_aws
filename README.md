# Two-tier architecture in AWS

Copy app to your AWS instance:

1. Open two terminals: one connect to your AWS instance with ssh connection, another one use for your local machine
2. In your terminal for local machine write the following command `scp -i "tech221.pem" -r "d:/VisualStudio Projects/virtualisation/app" ubuntu@c2-54-78-160-214.eu-west-1.compute.amazonaws.com:/home/ubuntu`, where:
    * `scp -i ` - run a secure copy command
    * `"tech221.pem" -r`- telling to use an ssh key, have to be inside the .ssh folder
    * `"d:/VisualStudio Projects/virtualisation/app"` - destination to the app folder
    * `ubuntu@c2-54-78-160-214.eu-west-1.compute.amazonaws.com:/home/ubuntu` - destination to the EC2 machine

Install required dependancies

1. `sudo apt-get install python-software-properties` - install python software properties

2. `curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -` - download a newer version of nodejs

3. `sudo apt-get install nodejs -y` - install nodejs on the EC2

4. `sudo npm install pm2 -g` - install pm2

Ensure SG allows port 3000

1. Select your instance and go into your Security Groups
2. Click on "Edit Inboud Rules"
3. Add a new rule:
    * Custom TCP
    * Set port to 3000
    * Source "IPv4 from anywhere (0.0.0.0)"
4. Save the rule

NPM install from the app folder location

1. `cd app` - navigate inside the app folder
2. `npm install` - install npm inside the app folder

NPM start

1. Run `node app.js` to start the app

App is available on port 3000

1. Copy the IPv4 address from your instance and paste it into the new tab in your browser
2. Add `:3000` at the end, so it looks like `http://54.78.160.214:3000/`
3. Your app should be running now

![App deployed](resources/app_deployed_on_EC2.JPG)


## Reverse Proxy

1. Use command `cd /etc/nginx/sites-available` in order to navigate inside the nginx configuration folder
2. Create a new configuration file with nano by using the following command: `sudo nano nodeapp.conf` (name could be anything, but try to make logical)
3. Inside the file type the following code:
```
server {
   listen 80;
   server_name <server name>;

   location / {
       proxy_pass http://<server name>:3000;
       proxy_set_header Host $host;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_set_header X-Forwarded-Proto $scheme;
   }
}
```
`<server name>` - your EC2 IP address

4. Use `ctrl+x` to exit nano, then press `y` to save the changes, and then press `enter` to save the name of the file
5. Enable the configuration by creating a symbolic link to enable a new config file: `sudo ln -s /etc/nginx/sites-available/nodeapp.conf /etc/nginx/sites-enabled/nodeapp.conf`
6. Check configuration for errors `sudo nginx -t`
7. Reload nginx - `sudo systemctl reload nginx`
8. Enable nginx - `sudo systemctl enable nginx`
9. Go back to your home folder by using `cd` command and then navigate in to `cd app`
10. Launch app in the backgroun by using `node app.js &`
11. Paste your ip, without port number, into browser and check if it works:

![Reverse Proxy](resources/reverse_proxy.JPG)

