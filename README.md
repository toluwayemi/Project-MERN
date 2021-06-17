# **SIMPLE TO-DO APPLICATION ON MERN WEB STACK**
### To deploy a simple to-do application that creates To-do list.
## BACKGROUND CONFIGURATION
* Ubuntu update   `sudo apt update`
* Upgrade Ubuntu `sudo apt upgrade`
* Install nodejs `sudo apt-get install -y nodejs`
    * $ mkdir Todo
    * $ cd Todo
    * use command `npm init`. This will install package.json
* Install ExpressJs `$ npm install express` 
* $ touch index.js
* Install the dotenv module `npm install dotenv`
* Open the index.js file `vim index.js`
* Type the code below :
``` 
        const express = require('express');
        require('dotenv').config();

        const app = express();

        const port = process.env.PORT || 5000;

        app.use((req, res, next) => {
        res.header("Access-Control-Allow-Origin",   "\*");
             res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
        next();
    });
         app.use((req, res, next) => {
    res.send('Welcome to Express');
    });

    app.listen(port, () => {
    console.log(`Server running on port ${port}`)
    });
```
* Start your server to see if it works `node index.js`
* Now open port 5000 on EC2 Security Groups
* Open your browser and access your server's public address or public DNS name using port 5000

        http://<PublicIP-or-PublicDNS>:5000


