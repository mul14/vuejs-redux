# redux-vuejs

Simple binding between Vue and Redux.
This allow to use multiple store if needed.
This binding is inpired by react-redux.
This work in inserting a Proxy component that is able to pass down the state, existing props and bounded actions to the child component.

Why it's good:

  - 37 lines of code (Easy to read/understand), easy to extend.
  - Same api as react-redux.
  - Combine multiple connect to be hydrated from multiple sources.
  - No hard coded dependencies between 'Vue' and the store, so more composable.
  - 0 dependency

// TODO
Check if events can be passed up through the parent

exemple:

ComponentContainer.js

```javascript
import { createStore, bindActionCreators } from 'redux';

import ChildComponent from 'child';

// for the exemple purpose, create dummy reducer and dummy store here.

function reducer(state = { foo: 'bar' }, action) {
  return state;
}

const store = createStore(reducer);

// like redux, create mapStateToProps and mapDispatchToProps
// the role of mapStateToProps is to map a part of the state to the child props.
// (maybe reselect or something like that would be good)

// here map all the store
function mapStateToProps(state) {
  return { propName: state };
}

// create our actions here (or import them from an other file)
function dummyAction1() {
  return { type: 'DUMMY1' };
}

function dummyAction2() {
  return { type: 'DUMMY2' };
}


// since we don't want to have a store instance into or component,
// we bind our actions with the dispatch function.

function mapDispatchToProps(dispatch) {
  return { actions: bindActionCreators({ dummy1, dummy2 }, dispatch);
}

export default connect(store)(mapStateToProps, mapDispatchToProps)(Child);
```

and our child

```vue


export default {
  props: ['actions', 'propName'],

  mounted() {
    this.actions.dummy1(); // action available
    this.propName; // contain our state
  }
}

```

Since the proxy component pass down the props to the child, we can create multiple proxy in order for the child to be hydrated by differents sources.

```
export default
  connect(store2)(mapStateToProps2, mapDispatchToProps2)(
    connect(store)(mapStateToProps, mapDispatchToProps)(Child));
```




