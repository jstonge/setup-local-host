# setup-local-host

## Goal

We want to setup the following for the Label Studio Enterprise platform:

 - Local storage to access non-text files (images, pdfs, ...)
 - Run inference and train models to do online learning and active learning from local machine

## Instructions

#### Accessing the local machine

 - Install [openssh server](https://www.openssh.com/) on local machine (`sudo apt install openssh-server`)
 - Check if it is running (`sudo systemctl status ssh`)
 - Open the firewall port on port 22 (default) (`sudo ufw allow ssh`)
 - Find the public ip address (`ip addr`)
 - Set up user accounts for each user
   - `sudo adduser username`
   - `sudo usermod -aG sudo username`
- Access from remote (ssh username@ipaddress), e.g.
```
ssh jstonge@192.168.1.137
```

 #### Setup a server using `nodejs` 
 
 - install [nodejs](https://nodejs.org/en/download) 
 - Create dir to have `expressjs` app
```
mkdir myapp
cd myapp
npm init -y # setup npm package
npm install express
touch server.js
```
 - inside server.js file
```
#server.js
const express = require('express')
const path = require('path')
const app = express()
const port = 3000 // can be any port. NGINX gonna use that port as local port

// Serve static files from the "public" directory
app.use(express.static(path.join(__dirname, 'public')));

// Start the server
app.listen(port, () => {
  console.log(`Server is running on http://localhost:${port}`);
});
```

#### Setup `nginx` reverse proxy (handle requests from other people)

 - allow incoming traffic
