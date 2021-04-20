# 20-04-21 Afternoon;  *Recursion, Async, Callbacks and  Promise*
useful links 
: [Recursion](#recursion), [Promises intro](#promises-intro), [Promise](#promise)
## Recursion
infinite loop
```js
function rec(x){
  console.log(x);  
    rec(x);
  }
}
rec(5);
```
with base case
```js
function rec(x){
  if(x==1){
  console.log(x);  
  } else{
    console.log(x);
    rec(x-1);
  }
}
rec(5);
```
this function will exist 5 times in the stack for each variation of x, i.e the amount of times you call a function == the amount of time it will exist in the stack

output
``` 
5
4
3
2
1
```
we can confirm the existence of copies by
```js
function rec(x){
  if(x==1){ 
    return 1;
  } else{
    return x + rec(x-1);
  }
}
var a = rec(5);
console.log(a); 
```
output
``` 
15
``` 

###### then sir went into recursion vs iteration and more specifically into why we use sqrt(n) while finding prime factor

## Promises intro

#### time example
lets see
```js
console.log('start');

setTimeout(()=>{
    console.log('abc')
},2000)

console.log('end')
```
output
``` 
start
end
abc
```

now 
```js
alert('start');

setTimeout(()=>{
    console.log('abc')
},2000)

alert('end')
```
if we close the alerts instantly or even if we wait the output is the same, its because of task scheduling
``` 
start
end 
abc
```

###### now onto a bit more practical examples
```js
var data = [1, 2, 3];
function printElement(){
    setTimeout(() => {
        data.forEach((e)=>{
            console.log(e);
        })
    },2000);
}

function insertElement(){
    setTimeout(() => {
        data.push(4);
        console.log(data);
    },3000);
}
insertElement();
printElement();
```
output after 2 sec
```
1
2
3
4
```
output after 3 sec
```
1
2
3
4
[ 1, 2, 3, 4 ]
```
now suppose you were retrieving some data from an api call in printElement() and it would take some time which was to be used in insertElement() then the data wouldn't be there and it would cause an error 

to solve it we use callback

we basically call the printElement after  insertElement is done doing its work

```js
var data = [1, 2, 3];
function printElement(){
    setTimeout(() => {
        data.forEach((e)=>{
            console.log(e);
        })
    },2000);
}

function insertElement(fun){
    setTimeout(() => {
        data.push(4);
        fun();
    },3000);
}
insertElement(printElement);
```
now the insertElement would call printElement after it is done executing and the output after 5 sec would be 
```
1
2
3
4
```

## Promise

basic flag syntax
```js
var doSomething = new Promise((res, rej) => {
    var flag = false;

    if (flag) {
        res('done');
    } else {
        rej('failed')
    }
});

doSomething.then((m) => {
    console.log(m + " task ")
}).catch((m) => {
    console.log(m + " task ")
})
```