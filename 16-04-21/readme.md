# 16-04-21, **
useful links:
: [Q1](#q1)

# Q1 
 implement update and delete functionality on yesterdays app

home.html
 ```html
 <!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Home</title>
    <script src="./fetch.js">
    </script>

</head>

<body>
    <h1>Homepage</h1>
    <h3>Create a post</h3>
    <form action='http://localhost:3000/post' method='POST'>
        Task: <input type='text' name='name'><br>
        Deadline: <input type='text' name='time'><br>
        <input type="submit" value="Submit" >
    </form>

    <h3>Update a post</h3>
    <form action='http://localhost:3000/update' method='POST'>
        Old Task: <input type='text' name='oname'><br>
        new Task: <input type='text' name='name'><br>
        new Deadline: <input type='text' name='time'><br>
        <input type="submit" value="Submit" >
    </form>
    <h3>Delete a post</h3>
    <form action='http://localhost:3000/delete' method='POST'>
        Task name: <input type='text' name='oname'><br>
        <input type="submit" value="Submit" >
    </form>
    <div id="todos">

    </div>
</body>

</html>
```
add this to your index.js
```js
app.post('/update', (req, res) => {
    var old = {
        name: req.body.oname,
    }
    var post = {
        name: req.body.name,
        time: req.body.time,
    }
    MongoClient.connect(url, (err, client) => {

        if (err) throw err
        db = client.db('demo');
        db.collection('todo').update(old, post, (err, res) => {
            if (err) throw err;

            client.close();
        })
    })
});

app.post('/update', (req, res) => {
    var old = {
        name: req.body.oname,
    }
    var post = {
        name: req.body.name,
        time: req.body.time,
    }
    MongoClient.connect(url, (err, client) => {

        if (err) throw err
        db = client.db('demo');
        db.collection('todo').update(old, post, (err, res) => {
            if (err) throw err;
            client.close();
        })
    })
    res.sendFile('./home.html', { root: './public' });
});
```
