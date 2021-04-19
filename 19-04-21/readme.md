# 16-04-21, *Js refresher*
useful links:
: [Callback](#callback), [forEach](#for-each)

code:
```js
function sum(a,b){
  console.log(arguments);
  return a+b;
}

var x= sum(10,'20',30);
console.log(x);
```
output
``` 
[object Arguments] {
  0: 10,
  1: "20",
  2: 30
}
"1020"
```
code:
```js 
function sum(a,b){
  return a+b;
}

var x= sum(10);
console.log(x);
```
output
```
NaN
```
code:
```js
function sum(a,b=20){
  return a+b;
}

var x= sum(10);
console.log(x);

```
output
```
30
```
# Callback

basic example
```js
function sum(a){
    a();
}
sum(function(){console.log('hello world')})
```
output:
```
hello world
```
### for each 
with func notation
```js
var x=[2,4,1,6,2]
x.forEach(function(e){
  console.log(e);
});

```
output
```
2
4
1
6
2
```
sum of array using foreach without using an external array
```js
//nahi ho paya lmao,this has many loopholes
var x=[2,4,1,6,2]
x.forEach(function(num) {
  var g=x[0];
  var end=x[x.length-1];
    x[0]+=num;
  if(num==x[x.length-1]){
    x[0]=x[0]-g;
  }
});
console.log(x[0]);
```
sorry bois, didn't feel like taking notes but I noted the topics and you can search them [here](https://developer.mozilla.org/en-US/), best of luck 
```js
1. array.map
2. array.filter
3. array.reduce
```