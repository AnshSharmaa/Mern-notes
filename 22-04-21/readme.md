# 22-04-21 Morning;  *Async js*
useful links 
: [Recursion](#recursion), [Promises intro](#promises-intro), [Promise](#promise)

## Intro

##### examples

```js
console.log('start');

setTimeout(()=>{
    console.log('mid')
},0);

console.log('end');
```
even with timeout 0 
output
```
start
end
mid
```
it could be that setTimeout take a few ms more to be complied so lets check with another example

```js
function A() {
    var i = 0;
    for (i = 1; i <= 100; i++) {

    }
    console.log('inside A');
}

function B() {
    var i = 0;
    for (i = 1; i <= 100; i++) {

    }
    console.log('inside B');
}

console.log('start');
setTimeout(() => {
    console.log('Mid');
}, 0);

A();
B();
console.log('end');
```
this time the output is
```
start
inside A
inside B
end
Mid
```
this is happening because of a thing called call stack(event loop)

now js runs in the js engine inside the browser and js is single threaded
the engine has a call stack and whenever a function is called its added in the stack, you can check out the call stack by addding break points in the dev tools of your browser

```js
function A() {
    var i = 10;
    console.log('A');
    B();
    console.log('A again');

}

function B() {
    console.log('B');
    C();
    console.log('B again');
}

function C() {
    console.log('C');
}

A();
```

the output would be 
```
A
B
C
B again
A again
```
so the stack will have
```
A
```
then 
```
B
A
```
then 
```
C
B
A
```
then
```
B
A
```
then
```
A
```
and the code ends

now
```js
function A() {

    console.log('A 1');


}

function B() {
    console.log('B 2');

}

function C() {
    console.log('C 3');
}

function D() {
    console.log('D4 ');
}

function slow(){
    setTimeout(D,0);
}
A();
slow();
B();
C();
```
the output will  be 
```
A 1
B 2
C 3
D 4
```
if you check the flow slow is called but its still doesn't put D() in the stack till the end that is because setTimeout is handled by a queue and it will be added to the event loop(An entity which adds stuff from queue to call stack) and event loop will add it back when the stack is empty thats why D 4 is printed at last 

so it doesn't matter what timeout you give to the function it is added when call stack is empty

## Aysnc 
Sir explained Async using [this](https://medium.com/swlh/asynchronous-javascript-explained-1aefc1304ac2) example






### then I had to dip out, will update this later