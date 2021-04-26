# 26-04-21 9am,  *React*

useful links : [Installation](#installation), [Hello world](#hello-world),[First Component](#first-component), [Adding css](#adding-css)

## Installation
to make a React app named demoapp we use
```  
npx creat-react-app demopapp 
```

this should take a while and when its done you will havbe a lot of files 
the important ones are the src and public

## Hello world
in your /src/App.js
replace all the code with
```js
import React from 'react';

class App extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello!</h1>
      </div>
    );
  }
}

export default App;

```
and in your /src/index.js
```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

ReactDOM.render(

    <App />
  ,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```

now you can open a terminal in demoapp and type
```
npm start 
```

## First component

now in /src make a folder named components and in it make 2 files ComponentOne.js and ComponentOne.css
 ComponentOne.js
```js
import React from 'react'

class ComponentOne extends React.Component {
    render() {
        return (
            <div>
                <h1>One</h1>
                <h1>One2</h1>
            </div>
        )
    }
}

export default ComponentOne;
```

notice how we added both h1 in div that's because React components can only return 1 parent tag that can have multiple tags

now in App.js
import ComponentOne by  
```js
import ComponentOne from './components/ComponentOne';
```
and replace \<App /> with
```js
<ComponentOne />
```

now if you refresh your browser it should diplay \<ComponentOne>


## Adding css
now in ComponentOne.js import the css file by <div className="demo">
```js
import './ComponentOne.css'
```
and to give class to parent div use 
```js
<div className="demo">
```
now in the css file
```css
.demo{

}
```
type any css in it and try it out

```js
<>
  <h1>One</h1>
  <h1>One2</h1>
</>
```
is valid syntax too


##

now open the app that we made in [15-04](https://github.com/AnshSharmaa/Mern-notes/tree/main/15-04-21)
and start the server and open the login page to check 
then make a new folder in the home dir of [15-04](https://github.com/AnshSharmaa/Mern-notes/tree/main/15-04-21) and in the folder open a terminal and type node "location of index.js"
```sh
node "location\index.js"
```
now check the browser again and you shouldn't be able to visit /login anymore

to check this in the /login remove the send file and replace it with 
```js
  console.log(__dirname);
  res.send('abc');
```

this should work now for static to work in your express.static
```js
app.use(express.static(path.join(__dirname,'public')));
```

this was to make static work when we run it from another dir which will come handy while deployment