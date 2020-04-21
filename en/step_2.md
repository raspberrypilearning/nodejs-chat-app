## Step title

Import required packages
```nodejs
const io = require('socket.io-client');
const readline = require('readline');
```

Link to the server
```nodejs
const socket = io("https://node-char-server.marcscott.repl.run");
```

Create a readline object for stdin and stdout
```nodejs
const rl = readline.createInterface({
	input: process.stdin,
	output: process.stdout
	});
```

When connected to server ask a question

```nodejs
socket.on('connect', () => {
	rl.question(`What's your name? `, (answer) => {
	});
	});
```

Send response back to server

```nodejs
socket.on('connect', () => {
	rl.question(`What's your name? `, (answer) => {
	socket.emit("message", `${answer} has joined the chat`)
	});
	});
```

Emit what the server transmits

```nodejs
socket.on('connect', () => {
	rl.question(`What's your name? `, (answer) => {
	socket.emit("message", `${answer} has joined the chat`)
	});

    socket.on('msg', function(data){
        console.log(data);
    })
	});
```

Create chat function to send message to server

```nodejs
function chat(){
    rl.question(`>> `, (answer) => {
        socket.emit("message", answer);
		chat();
    })
}
```

Add call to funtion in connect and in on
```nodejs
socket.on('connect', () => {
	rl.question(`What's your name? `, (answer) => {
	socket.emit("message", `${answer} has joined the chat`);
    chat()
	});

    socket.on('msg', function(data){
        console.log(data);
        chat()
    })
	});
```

Stop it repeating back to me all the time.

```nodejs
let my_message = ''

socket.on('connect', () => {
	rl.question(`What's your name? `, (answer) => {
	socket.emit("message", `${answer} has joined the chat`);
    chat()
	});

    socket.on('msg', function(data){
        if(my_message !=data){
        console.log(data);
        chat()};
    })
	});
```

ID the chatters

```nodejs
let id = ''

function chat(){
    rl.question(`>> `, (answer) => {
        socket.emit("message", `${id}: ${answer}`);
        my_message = answer;
        chat()
    })
}
```

Stop this repeating back
```nodejs
function chat(){
    rl.question(`>> `, (answer) => {
        socket.emit("message", `${id}: ${answer}`);
        my_message = `${id}: ${answer}`;
        chat()
    })
}
```
