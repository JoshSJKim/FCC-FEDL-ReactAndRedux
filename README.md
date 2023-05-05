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
        const messageArr = [...this.state.message, this.state.input];
        this.setState({
            input: '',
            messages: messageArr
        });
        event.preventDefault();
    }
    render() {
        const msg = this.state.messages.map((elem) => <li key={elem}>{elem}</li>);
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
        type: 'ADD',
        message: message
    }
};

const messageReducer = (state = [], action) => {
    switch(action.type) {
        case 'ADD':
            return state.concat(action.message);
        default: 
            return state;
    }
};

const store = Redux.createStore(messageReducer);
```