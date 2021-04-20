# 20-04-21 9am,  *Making a video chat app*

useful links : [Setup](#setting-up), [Server code](#making-a-room-with-unique-id), [Templating room](#templating-room), [Socketing](#socketing), [Front end Script](#front-end-scripts) , [End](#fin)
## Setting up

open a terminal and type 
```
npm install express uuid ejs socket.io nodemon
```
this gives us 
1. express(node's framework)
2. uuid is used to generate unique id's that will denote our room names
3. EJS is a simple templating language that lets you generate HTML markup with plain JavaScript
4. Socket.IO is a library that enables real-time, bidirectional and event-based communication between the browser and the server
basically it helps creating a backend socket it will help us in making other people join the rooms
5. so that we dont have to restart out server with each change

after the installation in your package.json replace the script part with 
```json
  "scripts": {
    "dev" : "nodemon server.js"
  },
```
now to start the server we have to use ```npm run dev```
## Making a room with unique id 
now make a file named server.js in the root of the dir
server.js
```js
const express = require('express');
const app = express();
const server = require('http').Server(app); // we made an external server and initialized it in app
const io = require('socket.io')(server); // passiong the server to socket io
const { v4: uuidV4 } = require('uuid'); //importing uuid

app.set('view engine', 'ejs'); // telling the server to use the ejs view engine(to create html with plain js)
app.use(express.static('public')); // telling the server to look into public folder for files

server.listen(3000, () => {
    console.log('running on 3000')
});

// http://localhost:3000/ will redir the user to a particular room(/:room)
app.get('/', (req, res) => {
    res.redirect(`${uuidV4()}`); // creating a new uuid
});

// now we are using a dynamic param room which will be generated from uuid
app.get('/:room', (req, res) => {
    res.render('room', { roomID: req.params.room });
})

```

if you visit ```http://localhost:3000/ ``` it should redir you to a room with its id 

## Templating room 

now make a folder named views in root dir 
and make a file named room.ejs

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Room</title>
    <style>
        /* we basically made little squares of 300x300 px and it will divide them automatically*/
        #video-grid {
            display: grid;
            grid-template-columns: repeat(autofill, 300px);
            grid-auto-rows: 300px;
        }
        /* styling the video to crop the extra part and keep the aspect ratio*/
        video {
            height: 100%;
            width: 100%;
            object-fit: cover;
        }
    </style>
</head>

<body>
    <div id='video-grid'>

    </div>
</body>

</html>
```

## Socketing

now in server.js add
```js
// making a socket and telling it what to do when there is a connection and when join-room
// this is in socket io syntax
io.on('connection', socket => {
    socket.on('join-room', (roodID, userId) => {
        console.log(roodID, userId);
    })
})
```

and this in 
room.ejs head 
```html
    <script>
        //var to get the current room id(this is the ejs syntax)
        const ROOM_ID='<%= roomID %>';
    </script>
```

now if you visit ```http://localhost:3000/ ``` and type ```ROOM_ID``` in the console it should print the current id which you can verify from the actual url you are at right now

## Front end scripts
in the room.ejs add these in the head tag
```html
    <script src='/socket.io/socket.io.js'></script>
    <script src='script.js' defer></script>
```
remember we told our server to sever to look into public folder for files? now it is time to make that make public in root dir and make a script.js file in it

```js
// this script runs when they enter the url and it fires an event(join-room)
const socket = io('/');

// this sends the roomid and userid(hardcoded) to the socket in out server side 
socket.emit('join-room', ROOM_ID, 10);
```

you can confirm that this is working by visiting the ```http://localhost:3000/ ``` and your room id with the user id would be in the node terminal



# Fin

that was all for part one you can check the whole code [here](https://github.com/AnshSharmaa/VideoChat-app-node-ejs)