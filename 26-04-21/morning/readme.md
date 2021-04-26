# 26-04-21 9am,  *Making a video chat app part 2*

useful links : [Making the join user message](#making-the-join-user-message), [Peer.js](#peerjs), [Connecting video to browser](#connecting-video-to-browser), [End](#fin)

## Brief
last time we made a room using uuid but we dont have a user id and we need a [webrtc](https://webrtc.org/) connection so for that we will use peerjs


## Making the join user message

in the server.js modify your io.on('connection') with this
```js
io.on('connection', socket => {
    socket.on('join-room', (roomID, userId) => {
        console.log(roomID, userId);
        socket.join(roomID); // joins+makes a room
        socket.to(roomID).emit('user-connected', userId) // broadcating a message when someone joins a room
    })
})
```
and in your script.js
```js
socket.on('user-connected', userId=>{
    console.log('User-Connected', userId)
})
```

## Peer.js

##### Installation 
to add it to our ejs file add
```html
<script src="https://unpkg.com/peerjs@1.3.1/dist/peerjs.min.js" defer></script>
```
and for our server 
``` npm i -g peer```

-g is important for webrtc connections because our resources(mic,cam,port) are global


##### Starting peer server
now open a new terminal and type 
``` peerjs --port 3001 ```

now that we have started the server we need to tell our code where it is so in script.js
```js
const myPeer = new peer(undefined,{
    host:'/',
    port: '3001',
})
```
here the undefined is to tell peer that it should give us self generated userID's

##### Checking userID
now if you rerun your server and open([locahost](http://localhost:3000/)) check in the peerjs terminal it should say 
``` 
Client connedted: $userId 
```
and if someone joins the same room the peer terminal would say
```
Client connected : $userID1 
```

and your broweser console should say 
```
User-Connected userID1
```


## Connecting video to browser
for this add these vars and functions in your script.js 
```js
const videoGrid = document.getElementById('video-grid');
const myVideo = document.createElement('video');
myVideo.muted = true


navigator.mediaDevices.getUserMedia({
    video: true,
    audio: true,
}).then(stream => {
    addVideoStream(myVideo, stream)

    socket.on('user-connected', userId => {
        connectToNewUser(userId, stream)
    })

    myPeer.on('call', call => {
        call.answer(stream);
    })
})

function connectToNewUser(userId,stream) {
    const call = myPeer.call(userId, stream);
    const video = document.createElement('video')

    call.on('stream', userVideoStream => {
        addVideoStream(video, userVideoStream);
    })
    call.on('close', () => {
        video.remove();
    })
}

function addVideoStream(video, stream) {
    video.srcObject = stream;
    //tells the function to play video after all the metadata is loaded
    video.addEventListener('loadedmetadata', () => {
        video.play();
    });
    videoGrid.appendChild(video);
}
```
now it look something like this
```js

// this script runs when they enter the url and it fires an event(join-room)
const socket = io('/');

const videoGrid = document.getElementById('video-grid');
const myVideo = document.createElement('video');
//so that we don't hear our own voice
myVideo.muted = true

// telling the peer to connect on port
const myPeer = new Peer(undefined, {
    host: '/',
    port: '3001',
})

// sets params for our video feed and sends it to our function(a)
navigator.mediaDevices.getUserMedia({
    video: true,
    audio: true,
}).then(stream => {
    addVideoStream(myVideo, stream)

    // calls connectToNewUser when user-connected event is fired
    socket.on('user-connected', userId => {
        connectToNewUser(userId, stream)
    })

    myPeer.on('call', call => {
        call.answer(stream);
    })
})

// this sends the roomid and userid to the socket in out server side 
myPeer.on('open', id => {
    socket.emit('join-room', ROOM_ID, id)
})

//adds the new user to video grid
function connectToNewUser(userId, stream) {
    const call = myPeer.call(userId, stream);
    const video = document.createElement('video')

    call.on('stream', userVideoStream => {
        addVideoStream(video, userVideoStream);
    })
    call.on('close', () => {
        video.remove();
    })
}

function addVideoStream(video, stream) {
    video.srcObject = stream;
    // tells the function to play video after all the metadata is loaded
    video.addEventListener('loadedmetadata', () => {
        video.play();
    });
    videoGrid.appendChild(video);
}
```
and in your server.js should look like 
```js
const { Socket } = require('dgram');
const express = require('express');
const app = express();
const server = require('http').Server(app); // we made an external server and initialized it in app
const io = require('socket.io')(server); // passiong the server to socket io
const { v4: uuidV4 } = require('uuid'); //importing uuid

app.set('view engine', 'ejs'); // telling the server to use the ejs view engine(to create html with plain js)
app.use(express.static('public')); // telling the server to look into public folder for files

// http://localhost:3000/ will redir the user to a particular room(/:room)
app.get('/', (req, res) => {
    res.redirect(`${uuidV4()}`); // creating a new uuid
});

// now we are using a dynamic param room which will be generated from uuid
app.get('/:room', (req, res) => {
    res.render('room', { roomID: req.params.room });
})

// making a socket and telling it what to do when there is a connection
// this is in socket io syntax
io.on('connection', socket => {

    socket.on('join-room', (roomId, userId) => {
        socket.join(roomId); // joins+makes a room
        socket.to(roomId).emit('user-connected', userId) // broadcating a message when someone joins a room
    })
})

server.listen(3000, () => {
    console.log('running on 3000')
});
```
## Fin 
with this part 2 is over and you should be able to see feeds on your web browser
you can check the whole code [here](https://github.com/AnshSharmaa/VideoChat-app-node-ejs)
