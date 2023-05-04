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
class DislayMessages extends React.Component {
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