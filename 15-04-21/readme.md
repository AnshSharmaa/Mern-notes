#15-04-21, *Connecting node to Mongo*
useful links:
: [Q1](#q1), [Q2](#q2), [Q3](#q3)

# opening mongo
open cmd and type 
```sh
cd C:\Program Files\MongoDB\Server\4.4\bin
./mongo
```

# Mongo demo 
of mongo connection

```js 
const MongoClient = require('mongodb');
// url where mongo is accessible,this can be found in the first few lines when you  run mongo in cmd
var url = 'mongodb://127.0.0.1:27017'


MongoClient.connect(url, (err, client) => {
    var data = {
        userid:101,
        username:'a',
    }

    if(err) throw err
    db = client.db('demo');
    db.collection('users').insertOne(data, (err,res)=>{
        if(err) throw err;

        console.log('data inserted');
        client.close();
    })
})
```

# Q1 
implement sign up functionality using mongo

signup.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>login</title>
</head>

<body>
    <form action='http://localhost:3000/signup' method='POST'>
        First Name: <input type='text' name='fn'><br>
        last Name: <input type='text' name='ln'><br>
        Username: <input type='text' name='un'><br>
        <input type="submit" value="Submit">

    </form>
</body>

</html>
```
index.js
```js
const express = require('express');
const app = express();
const fs = require('fs');
const MongoClient = require('mongodb');
const url = 'mongodb://127.0.0.1:27017'

app.use(express.urlencoded({ extended: false }));
app.use(express.static('public'));

app.get('/signup', (req, res) => {
    res.sendFile('./signup.html', { root: './public' });
});

app.post('/signupreq', (req, res) => {
    var user = {
        firstName: req.body.fn,
        LastName: req.body.ln,
        UserName: req.body.un,
    }
    console.log(user);
    MongoClient.connect(url, (err, client) => {

        if (err) throw err
        db = client.db('demo');
        db.collection('users').insertOne(user, (err, res) => {
            if (err) throw err;

            console.log(user);
            client.close();
        })
    })
});


app.get('/home', (req, res) => {
    res.sendFile('./home.html', { root: './public' });

});

app.listen(3000, () => {
    console.log('server started at 3000')
});
```

# Q2
implement login functionality using mongo

login.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>login</title>
</head>

<body>
    <form action='http://localhost:3000/login' method='POST'>
        UserName: <input type='text' name='un'><br>
        First name: <input type='text' name='fn'><br>
        <input type="submit" value="Submit">

    </form>
</body>

</html>
```
index.js
```js
const express = require('express');
const app = express();
const path = require('path');
const fs = require('fs');
const MongoClient = require('mongodb');
const url = 'mongodb://127.0.0.1:27017'

app.use(express.urlencoded({ extended: false }));
app.use(express.static('public'));


app.get('/login', (req, res) => {
    res.sendFile('./login.html', { root: './public' });
});

app.get('/signup', (req, res) => {
    res.sendFile('./signup.html', { root: './public' });
});

app.post('/signupreq', (req, res) => {
    var user = {
        firstName: req.body.fn,
        LastName: req.body.ln,
        UserName: req.body.un,
    }
    console.log(user);
    MongoClient.connect(url, (err, client) => {

        if (err) throw err
        db = client.db('demo');
        db.collection('users').insertOne(user, (err, res) => {
            if (err) throw err;

            console.log(user);
            client.close();
        })
    })
});

app.post('/login', (req, res) => {
    var user = {
        firstName: req.body.fn,
        UserName: req.body.un,
    }
    MongoClient.connect(url, (err, client) => {

        if (err) throw err
        db = client.db('demo');

        db.collection('users').findOne(user, (err, res) => {
            if (err) throw err;

            console.log(res);
            client.close();
        })
    })
});


app.get('/home', (req, res) => {
    res.sendFile('./home.html', { root: './public' });

});

app.listen(3000, () => {
    console.log('server started at 3000')
});
```


# Q3
make a post/todo adding functionality at homepage
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Home</title>
</head>

<body>
    <h1>Homepage</h1>

    <form action='http://localhost:3000/post' method='POST'>
        Task: <input type='text' name='name'><br>
        Deadline: <input type='text' name='time'><br>
        <input type="submit" value="Submit">

    </form>
</body>

</html>
```
index.js
```js
const express = require('express');
const app = express();
const path = require('path');
const fs = require('fs');
const MongoClient = require('mongodb');
const url = 'mongodb://127.0.0.1:27017';

app.use(express.urlencoded({ extended: false }));
app.use(express.static('public'));


app.get('/login', (req, res) => {
    res.sendFile('./login.html', { root: './public' });
});

app.get('/signup', (req, res) => {
    res.sendFile('./signup.html', { root: './public' });
});

app.post('/signupreq', (req, res) => {
    var user = {
        firstName: req.body.fn,
        LastName: req.body.ln,
        UserName: req.body.un,
    }
    MongoClient.connect(url, (err, client) => {

        if (err) throw err
        db = client.db('demo');
        db.collection('users').insertOne(user, (err, res) => {
            if (err) throw err;
            client.close();
        })
    })
});

app.post('/login', (req, res) => {
    var user = {
        firstName: req.body.fn,
        UserName: req.body.un,
    }
    MongoClient.connect(url, (err, client) => {

        if (err) throw err
        db = client.db('demo');

        db.collection('users').findOne(user, (err, res) => {
            if (err) throw err;

            console.log(res);
            client.close();
        })
    })
});

app.post('/post', (req, res) => {
    var post = {
        name: req.body.name,
        time: req.body.time,
    }
    MongoClient.connect(url, (err, client) => {

        if (err) throw err
        db = client.db('demo');
        db.collection('todo').insertOne(post, (err, res) => {
            if (err) throw err;

            console.log(post);
            client.close();
        })
    })
});

app.get('/home', (req, res) => {
    res.sendFile('./home.html', { root: './public' });

    MongoClient.connect(url, (err, client) => {

        if (err) throw err
        db = client.db('demo');

        db.collection('todo').find(user, (err, res) => {
            if (err) throw err;
            console.log(res);
            client.close();
        })
    })
/*  since node is server side and its window is in the console we cant use document variable 
    var user = {
    }
    MongoClient.connect(url, (err, client) => {

        if (err) throw err
        db = client.db('demo');

        db.collection('todo').find(user, (err, res) => {
            if (err) throw err;
            res.forEach(element => {
               build(element); 
            });
            client.close();
        })
    })
    function build(e){
        var main = document.getElementById("todos");
        //ndiv houses result of a day
        var ndiv = document.createElement("div");
        ndiv.classList.add("ndiv");
        ndiv.innerHTML = `<span>Name: ${e.name}</span><span>Deadline: ${e.time}</span>`
        main.appendChild(ndiv);
    }  */
});


app.listen(3000, () => {
    console.log('server started at 3000')
});
```