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
    - setting state is asynchronous
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