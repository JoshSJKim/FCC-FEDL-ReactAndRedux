# FCC-FEDL-ReactAndRedux

## Getting Started with React Redux

- React is a view library that you provide with data, then it renders the view in an efficient, predictable way.
- Redux is a state management framework that can be used to simplify the management of the application's state.
- Typically, in a React Redux app, a single Redux store is created to manage the state of the entire app.
- The React components then subscribe only to the pieces of data in the store that are relevant to their role.
- Then, actions are dispatched directly from React components, which trigger store updates.

- Although React components are capable of managing state on its own locally, it's better to keep the app state in a single location with Redux for complex apps.
- There are exceptions when individual components may have local state specific only to them.

- Redux is not designed to work with React 'out of the box'.
- `react-redux` package is required to pass Redux `state` and `dispatch` to React components as `props`.

```jsx
class DisplayMessages extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            input: '',
            messages: []
        }
    };
    render() {
        return <div />
    }
};
```

## Manage State Locally First

```jsx
class DisplayMessages extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            input: '',
            messages: []
        }
        this.handleChange = this.handleChange.bind(this);
        this.submitMessage = this.submitMessage.bind(this);
    }
    handleChange(event) {
        this.setState({
            input: event.target.value
        });
    }
    submitMessage(event) {
        const messageArr = [...this.state.message, this.state.input];  // Or state.messages.concat(state.input)
        this.setState({
            input: '',
            messages: messageArr
        });
        event.preventDefault();
    }
    render() {
        const msg = this.state.messages.map((elem, index) => <li key={index}>{elem}</li>);
        return (
            <div>
                <h2>Type in a new Message:</h2>
                <input value={this.state.input} onChange={this.handleChange} />
                <button type="submit" onClick={this.submitMessage}>Add Message</button>
                <ul>{msg}</ul>
            </div>
        );
    }
};
```

- The above code is managing state locally because it defines a 'state' object within its 'constructor' method, which is specific to this component instance.

```jsx
render() {
    const msg = this.state.messages.map((elem) => <li key={elem}>{elem}</li>);
    return (
        <div>
            <h2>Type in a new Message:</h2>
            <form onSubmit={this.submitMessage}>
                <input value={this.state.input} onChange={this.handleChange} />
                <button type="submit">Add Message</button>
            </form>
            <ul>{msg}</ul>
        </div>
    );
}
```

- by wrapping the `input` and `button` elements in a `<form>` element with an `onSubmit` handler that calls the `submitMessage` method, the message could be submitted by either clicking the 'Add Message' button, or simply by pressing the 'Enter' key.

## Extract State Logic to Redux

```jsx

const ADD = 'ADD';

const addMessage = (message) => {
    return {
        type: ADD,
        message: message
    }
};

const messageReducer = (state = [], action) => {
    switch(action.type) {
        case ADD:
            return state.concat(action.message); // Or [...state, action.message]
        default: 
            return state;
    }
};

const store = Redux.createStore(messageReducer);
```

## Use Provider to Connect Redux to React

- React requires access to the Redux store and the actions it needs to dispatch updates.
- React Redux provides a small API with two key features.
- First is the `Provider`, which is a wrapper component from React Redux that wraps the React app.
- It allows access to the Redux `store` and `dispatch` functions throughout the component tree.
- It takes two props: the Redux `store` and the child components of the app.

```jsx
<Provider store={store}>
    <App/>
</Provider>
```

```jsx
// Redux:

const ADD = 'ADD';              // action type

const addMessage = (message) => {   // action creator
    return {
        type: ADD,
        message: message
    }
};

const messageReducer = (state = [], action) => {
    switch (action.type) {
        case ADD:
            return [...state, action.message];
        default: 
            return state;
    }
};

const store = Redux.createStore(messageReducer);

// React:

class DisplayMessages extends React.Component {
    constructor(props) {
        super(props) ;
        this.state = {
            input: '', 
            messages: []
        }
        this.handleChange = this.handleChange.bind(this);
        this submitMessage = this.submitMessage.bind(this);
    }
    handleChange(event) {
        this.setState({
            input: event.target.value
        });
    }
    submitMessage(event) {
        const currentMessage = [...this.state.message, this.state.input];
        this.setState((state) => {
            return {
                input: '', 
                messages: currentMessage
            };
        });
    }
    render() {
        const msg = {this.state.messages.map((message, index) => {
            return(
                <li key={index}>{message}</li>
            )
        })};
        return(
            <div>
                <h2>Type in a new Message:</h2>
                <input value={this.state.input} onChange={this.handleChange} />
                <br />
                <button onClick={this.submitMessage}>Add Message</button>
                <ul>{msg}</ul>
            </div>
        );
    }
};

const Provider = ReactRedux.Provider; 
// Note: React Redux is available as a global variable, which means the Provider can be accessed using a dot notation, and assigned to a const variable, as shown above.

class AppWrapper extends React.Component {
    constructor(props) {
        super(props);
    }
    render() {
        return{
            <Provider store={store}>
                <DisplayMessages />
            </Provider>
        };
    }
};
```

## Map State to Props

- The `Provider` component allows the provision of `state` and `dispatch` to React components, but the exact state and actions must be specified.
- It ensures that each component only has access to the state it needs.
- Create two functions to accomplish this: `mapStateToProps()` and `mapDispatchToProps()`

- In these functions, declare which pieces of state are accessible and which action creators will be available for dispatch.

```jsx
const state = [];

const mapStateToProps = (state) => {
    return {
        messages: state
    }
};
```

- function `mapStateToProps()` takes `state` as an argument and returns an object that maps the state passed to specific property names. (in the case above, state is passed and set to the property 'messages')
- These properties will become accessible to the components via `props`.
