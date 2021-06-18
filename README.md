# **SIMPLE TO-DO APPLICATION ON MERN WEB STACK**
### Aim is to deploy a simple to-do application that creates To-do list.
## __Step 1 - Background Configuration__
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


![Port](port5000.PNG)


        http://<PublicIP-or-PublicDNS>:5000
![Welcome to Express](welcomeexpress.PNG)
# __ROUTES__
Our To-do application needs to perform three tasks as follows:

    * Create a new task
    * Display list of all tasks
    * Delete a completed task
We will use different standard `HTTP request methods`: POST , GET and DELETE.
For each task, we need to create three `routes`.
    mkdir routes
    cd routes
    touch api.js
    vim api.js
copy and paste below code

        ```
        const express = require ('express');
        const router = express.Router();

        router.get('/todos', (req, res, next) => {

        });

        router.post('/todos', (req, res, next) => {

            });

        router.delete('/todos/:id', (req, res, next) => {

        })

        module.exports = router;
    ```
# __MODELS__
We will model to define database schema. To create a Schema and a model, install `mongoose `which is a node.js package that makes working with `Mongodb` easier. 

        npm install mongoose

* mkdir models
* cd models
* todo.js
* vim todo.js

Copy and paste the following code.
    ```
        const mongoose = require('mongoose');
        const Schema = mongoose.Schema;

        //create schema for todo
        const TodoSchema = new Schema({
        action: {
        type: String,
        required: [true, 'The todo text field is 
        
        required']
        }
        })

        //create model for todo
        const Todo = mongoose.model('todo', TodoSchema);

        module.exports = Todo;
    ```

Now, we need to update the file `api.js` with the following code. 
    ```
        const express = require ('express');
        const router = express.Router();
        const Todo = require('../models/todo');

        router.get('/todos', (req, res, next) => {

        //this will return all the data, exposing only the id and action field to the client
        Todo.find({}, 'action')
        .then(data => res.json(data))
        .catch(next)
        });

        router.post('/todos', (req, res, next) => {
        if(req.body.action){
        Todo.create(req.body)
        .then(data => res.json(data))
        .catch(next)
        }else {
        res.json({
        error: "The input field is empty"
        })
        }
        });

        router.delete('/todos/:id', (req, res, next) => {
        Todo.findOneAndDelete({"_id": req.params.id})
        .then(data => res.json(data))
        .catch(next)
        })

        module.exports = router;
    ```

# __MongoDB Database__
We will use __mLab__. mLab provides MongoDB database as a service solution (DBaas).

![Mongoose](mongoose.PNG)


In the `index.js` file, we specified `process.env` to access environment variables, but we have not yet created this file. So we need to do that now.

Create a file in your Todo directory and name it      `.env`.

        touch .env
        vi .env

Add the connection string to access the database in it, just as below:

`DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'`

Update the content in `index.js` file with the following.
```
        const express = require('express');
        const bodyParser = require('body-parser');
        const mongoose = require('mongoose');
        const routes = require('./routes/api');
        const path = require('path');
        require('dotenv').config();

        const app = express();

        const port = process.env.PORT || 5000;

        //connect to the database
        mongoose.connect(process.env.DB, {      
        useNewUrlParser: true, useUnifiedTopology: 
        true })
        .then(() => console.log(`Database connected 
        successfully`))
        .catch(err => console.log(err));

        //since mongoose promise is depreciated, we 
        overide it with node's promise
        mongoose.Promise = global.Promise;

        app.use((req, res, next) => {
        res.header("Access-Control-Allow-Origin", 
        "\*");
        res.header("Access-Control-Allow-Headers", 
        "Origin, X-Requested-With, Content-Type, 
        Accept");
        next();
        });

        app.use(bodyParser.json());

        app.use('/api', routes);

        app.use((err, req, res, next) => {
        console.log(err);
        next();
        });

        app.listen(port, () => {
        console.log(`Server running on port ${port}`)
});
```

Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the `index.js` application file.

Start your server using the command:

        node index.js


You shall see a message ‘Database connected successfully’, if so - we have our backend configured.

![Database Connection](database.PNG)


## __Testing Backend Code using RESTFUL API__

I used postman to test API. 

Open postman, create a __POST__ request to the API.

        http://<PublicIP-or-PublicDNS>:5000/api/todos

![Postman](postman.PNG)

Create a __GET__ request to your API on 

        http://<PublicIP-or-PublicDNS>:5000/api/todos

![Postman](postman1.PNG)


## __Step 2 - Frontend Creation__

To start out with the frontend of the To-do app, we will use the `create-react-app` command to scaffold our app.

        npx create-react-app client

### __Running a React App__
We need to install some dependencies like `concurrently` and `nodemon`.

        "proxy": "http://localhost:5000"

![React Page](reactjs.PNG)

### __Creating your React Components__
 Make directory named `component` and create three files `Input.js` `ListTodo.js` and  `Todo.js` in this directory.

 Install Axios with  __*npm install axios*__.

Go back to Todo directory and run the following command.

        npm run dev




