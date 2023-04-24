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
