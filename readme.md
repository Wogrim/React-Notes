# Important

- React is a JavaScript library for creating Single Page Applications

## Getting Started

You *can* use React from a CDN and write plain JavaScript to do things  
(React will replace the contents of #root with a new h1 element after 3 second delay)
```
<body>
    <div id="root">
        <h3>React page rendering...</h3>
    </div>

    <script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
    <script>
        const domContainer = document.querySelector('#root');
        const root = ReactDOM.createRoot(domContainer);
        setTimeout(()=>{root.render(React.createElement("h1", {}, "React page has rendered"));},3000)
    </script>
</body>
```

But there's a better way to visualize the HTML elements you're creating, which lets you write HTML in your JavaScript (takes more setup)

install LTS version of Node.js from [https://nodejs.org/en/] (global install)

install **nodemon** for auto restart when you save changes to files (global install)
```
npm install -g nodemon
```

make a folder for your project and run  
(this downloads a whole bunch of react stuff ~200 MB)
```
npx create-react-app project-name
```

- **public/index.html** is a blank HTML with a #root where all the React content will be placed
- **src/index.js** renders the `<App />` component into #root
- **src/App.js** defines what the `<App />` component contains (mixture of JavaScript and HTML)
  - a few HTML things must be named different because of conflict with JavaScript: [https://reactjs.org/docs/dom-elements.html]

from the project folder run `npm start` to run the app
- it will be on **localhost:3000** (go there in browser)
- if you change things in App.js and save, the page will auto reload
- Ctrl+C to stop the app

## making your own components

you can make your own **components** folder under *src* to put your own components in
- a component file should be called **ComponentName.jsx** for *JavaScript React* syntax highlighting in VS Code
- import and use your components in *App* or in other components as `<ClassName something={47}/>`
  - (*something* is put in an object called **props**, can just use quotes if the value is a string)
  - you can also make a separate closing tag instead of self-closing, and put **children** elements in-between
- add the following code into your VS Code's settings.json for React emmet abbreviations:
```
    "emmet.includeLanguages": {
        "javascript": "javascriptreact",
    }
```

## class components

you can make your components as classes

in your component file put
```
import React, { Component } from 'react';

class ClassName extends Component {
  constructor(props){
    super(props);
    this.state={
      athing: 0
    };
  }
  render() {
    const something = this.props.something;
    return (
        <div>
          <h1>Something: {this.something}</h1>
          <h2>Athing: {this.state.athing}</h2>
          {this.props.children}
        </div>
    );
  }
}
export default ClassName;
```

- **this.state** is a special immutable object for keeping track of this component's state
  - to change state, you must use a special method to set the whole thing (use spread to copy the existing state):
    - `this.setState({...this.state, athing = this.state.athing + 1});`
    - setting state is asynchronous; it is not instant
    - changing state causes the component to re-render
- **render()** returns the HTML that will be in the page
- **return ()** allows you to return one element (which can have multiple elements inside it)
- {} is the portal to JavaScript which lets you put variables in the HTML
- if you need to return multiple things not inside something else, put them in a *fragment*
  - `<> <li>item1</li> <li>item2</li> <li>item3</li> </>`

## functional components

writing React components as functions instead of classes has become popular, so here's what that looks like:
```
import React, {useState} from 'react';

const ComponentName = (props) => {
  //const [variable, setVariable] = useState(startingValue);
  const [athing, setAthing] = useState(50);

  const changeAthing = () => {
    setAthing(athing+1);
  }

  return (
    <div>
          <h1>Something: {props.something}</h1>
          <h2 onClick={changeAthing}>Athing: {this.state.athing}</h2>
          {props.children}
    </div>
    );
}

export default ComponentName
```

## creating elements/components from an array

to create things based on array, you must use array.map()
- what you make needs a unique key; index of the element is fine
```
<div>
{
  colors.map((color,index) => { return (
    <div key={index}>
      <h3>This color is {color}</h3>
    </div>
  );})
}
</div>
```

making a component instead (pass info it needs via props)
```
<div>
{
  colors.map((color,index) => { return (
    <Box key={index} color=color />
  );})
}
</div>
```

## conditional rendering

use javascript ternary operator if you want to have logic decide whether to render something
- (based on a variable in state)
```
// if errorMessage is not empty string (empty string is falsey), show it, else nothing
{
  errorMessage ?
  <h3 style={styles.error}>{errorMessage}</h3>
  : ''
}
```

## React Sythetic Events

add events to elements in your custom components
- slightly different version of regular JavaScript events
- full list [https://reactjs.org/docs/events.html#supported-events]

call an arrow function you define on the spot:
```
<button onClick={ () => { this.setState({ ...this.state, athing: this.athing + 1 }) } }>increase athing</button>
```

or add an arrow method to your class and call it (non-arrow method doesn't seem to have access to *this*):
```
//method
increaseAthing = () => {
  this.setState({...this.state, athing = this.state.athing + 1});
}
//...
<button onClick={this.increaseAthing}>increase athing</button>
```

## form-specific stuff

keeping track of input changes on a form (functional)
- double-binding keeps the input value in sync with state
```
//inside the component's function
const changeSize = (newSize) => {
  setSize(newSize);
  //do some validation if you want, but must use newSize because size is not yet updated
}
//...
<input type="number" onChange={(e)=>changeSize(e.target.value)} value={size}>
//or if not doing anything extra, directly use setSize() and you don't need the extra function
<input type="number" onChange={(e)=>setSize(e.target.value)} value={size}>
```

prevent the form submission from refreshing the page (default behavior)
```
//inside the component's function
const handleSubmit = (e) => {
  e.preventDefault
  //do whatever else the form submission should do, using the variables from state
}
//...
<form onSubmit={handleSubmit}>
```

checkboxes don't have a value, they have a *checked* property (boolean)
```
<input type="checkbox" checked={complete} onChange={(e) => setComplete(e.target.checked)} />
```

select and options:
```
const [selected, setSelected] = useState(startingID);
//...
<select value={selected} onChange={(e) => setSelected(e.target.value)}>
    {
        items.map((item, index) => (
            <option key={index} value={item.id}>{item.name}</option>
        ))
    }
</select>
```

## sending data from child to parent components ("lifting state")

to send data from parent to child components, we send them as props

for a parent to get back data from its child, it must pass a function to the child (in props)
```
//in parent component
const [items, setItems] = useState([]);

const addTask = (label) => {
    setItems([...items, { label: label, complete: false }]);
}
//...
<TodoListForm addTask={addTask} />

//in child component ('label' is in state, from an input)
const submitForm = (e) => {
    e.preventDefault();
    props.addTask(label);
}
```

## useEffect - do something on component first render

import it with useState
```
import React, {useState, useEffect} from 'react';
```

in your functional component, call the **useEffect** function
- first parameter is function that will be run on first render of the component
- second parameter is an array of variables to watch: when one changes, run first parameter again
- if first parameter returns a function, it will be called when the component is being destroyed (use for any needed cleanup)
```
useEffect(() => {
  fetch("https://pokeapi.co/api/v2/pokemon?limit=-1")
    .then(response => response.json())
    .then(response => {
        setAllPokemon(response.results.map(p => p.name));
    })
    .catch(err => {
        setAllPokemon([]);
    });
}, []);
```

## Axios for easier API calls

in your project folder (adjacent to *package.json*) with the server not running
```
npm install axios
```

in whatever component you need to use it
```
import axios from 'axios';
```

to make an API get request
```
fetch("https://pokeapi.co/api/v2/pokemon?limit=-1")
  .then(response => response.json())
  .then(response => {
      setAllPokemon(response.results.map(p => p.name));
  })
  .catch(err => {
      setAllPokemon([]);
  });
//BECOMES
axios.get("https://pokeapi.co/api/v2/pokemon?limit=-1")
  .then(response => {
      setAllPokemon(response.data.results.map(p => p.name));
  })
  .catch(err => {
      setAllPokemon([]);
  });
```

- saves us from doing `.then(response => response.json())`
- the actual server response is now in *response.data*

## front-end routing with React Router

front-end routing is the front-end use of the URL to change the components of the page  
(as opposed to the usual back-end routing where the server gives you a different page based on the URL)

to install React Router
```
npm install react-router-dom
```

in **index.js**
```
import {BrowserRouter} from 'react-router-dom';
// in the render, wrap <App /> in a <BrowserRouter> component
<BrowserRouter>
  <App />
</BrowserRouter>
```

in **App.js**
```
import {Routes,Route} from 'react-router-dom';
//in the render, each Route specifies what is displayed if the URL matches its path
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
<Routes>
```

wherever you need front-end routing links, don't use *a* tag, use **Link**
```
import {Link} from 'react-router-dom';
//in the render
<Link to="/about">About Page</Link>
```

if we want to navigate someone to a route such as in response to an event
```
import {useNavigate} from 'react-router-dom';
//near the top of our functional component
const navigate = useNavigate();
//to do the navigation in a function somewhere
navigate(`/people/${id}`);
```

you can also `navigate(-1)` to send the user back one page or `navigate(1)` for forward


if your routes need to get a variable in the path
```
import {useParams} from 'react-router';
//near the top of our functional component
const {personID} = useParams();
//in the Route's path, precede variable name with colon to specifiy it is a variable
<Route path="/people/:personID" element={<Person />} />
```
