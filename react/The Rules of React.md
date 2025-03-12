출처:(https://gist.github.com/sebmarkbage/75f0838967cd003cd7f9ab938eb1958f)
# The Rules of React

All libraries have subtle rules that you have to follow for them to work well. Often these are implied and undocumented rules that you have to learn as you go. This is an attempt to document the rules of React renders. Ideally a type system could enforce it.

# What Functions Are "Pure"?

A number of methods in React are assumed to be "pure".

On classes that's the constructor, getDerivedStateFromProps, shouldComponentUpdate and render.

```js
class MyComponent extends React.Component {
  constructor() {
    // pure
  }
  
  static getDerivedStateFromProps() {
    // pure
  }

  shouldComponentUpdate() {
    // pure
  }

  render() {
    // pure
  }
}
```

However, componentDidMount, componentDidUpdate, componentWillUnmount and custom event handlers are *not* required to be pure.

```js
class MyComponent extends React.Component {
  componentDidMount() {
    // not pure
  }
  componentDidUpdate() {
    // not pure
  }
  componentWillUnmount() {
    // not pure
  }
  handleClick = () => {
    // not pure
  }
  render() {
    // pure
    return <div onClick={this.handleClick} />;
  }
}
```

The first argument to setState has to be a pure function.

```js
this.setState(() => {
  // pure
});
```

But *not* the second argument.

```js
this.setState({}, () => {
  // not pure
});
```

Functional components are also pure functions.

```js
function MyComponent() {
  // pure
}
```

### Getters in State

setState also has another specific limitation. The object passed to setState or returned from the first argument cannot have any getters on it that might break any of the rules above. This is because they're also evaluated as part of a pure function.

# What Does "Pure" Mean in React?

Pure in general has a specific meaning but can vary by definition. In React, it mostly means that it has to be idempotent. It will always return the same thing for the same input.

## Forbidden

Specifically this means that within those functions you cannot:

- Mutate a variable binding, unless that binding was "newly created".
- Mutate a property on an object, unless that object was "newly created".
- Mutate a property on `this` except in the constructor.
- Read a property from an object, unless that object was "newly created" or if the property will never change after it is read once.
- Read a variable binding, unless that binding was "newly created" or if its will never change after it is read once.
- Call `Math.random()` or `Date.now()` since these read values that later change.
- Call `setState`, since this mutates.
- Issue a network request (POST), file system or other I/O that writes to a persistent store. E.g. impression logging, create, updates or deletes.
- Create a new component type. A new function/class that is later used in JSX.
- Call another function that might do anything of the above.

"Newly created" above means that the object, or the closure around a variable binding, was created inside the pure function call itself. Note that it is not enough that this is an object created by a previous invokation of this function. It has to be within each call to the function.

## Allowed

There are a few things you might not consider as pure according to other definitions but which are ok in React:

- Reading `this.props` or `this.state` in class components is ok even though they change over time.
- Throwing errors or any other object is ok.
- Mutating objects is ok as long as that object is "newly created".
- Mutating bindings is ok as long as that binding was "newly created".
- You can call a function which mutates an object you pass in, as long as that object is "newly created".
- Mutating properties on `this` is ok in the constructor because it is "newly created" in class constructors.
- Creating new functions that are not pure is ok, as long as they're not called until later, outside a pure function.
- Issuing a network request (GET), a read from the file system or other I/O is ok if this doesn't cause a write, and as long as the result is not read by the pure function*.

*) Why would you load something you're not going to read? It can be used to prime a cache and the read is from the cache.

### Lazy Initialization

An exception to these rules is that it is ok to read and mutate a value if it is for the purpose of lazy initialization. By that we mean that you only read to check if a property/binding is initialized, if not initialize it, write the result to the property/binding and from that point always return the same value.

```js
var lazy;
function MyComponent() {
  if (!lazy) {
    lazy = function() { }; // ok
  }
  return lazy; // ok
}
```

This can also be used with multiple values in a Map as long as there is a unique key.

```js
var cache = new Map();
function MyComponent(props) {
  if (!cache.has(props.id)) {
    cache.set(props.id, function() { }); // ok
  }
  return cache.get(props.id); // ok
}
```

If a value is not yet ready to be initialized, you can throw a Promise that resolves when it is ready to be initialized.

Once a value has been initialized, it can never change again. If you want to invalidate a cache, you can store the cache itself in "state".

# Mutating Objects Created by Render

An additional restriction is that you cannot mutate objects or closure bindings created in any of these functions, after the render completes. They are implicitly frozen. That includes objects, and closures. The exception to this is mutable objects stored in the state object.

```js
function Foo() {
  let x = {active: false};
  let y = 0;
  let clickHandler = () => {
    x.active = true; // not ok
    y++; // not ok
  };
  return <div onClick={clickHandler} />;
}
```

```js
class Component extends React.Component {
  state = { mutableBox: { active: false, count: 0 } };
  clickHandler = () => {
    this.state.mutableBox.active = true; // ok, but discouraged
    this.state.mutableBox.count++; // ok, but discouraged
    this.forceUpdate();
  };
  render() {
    return <div onClick={this.clickHandler} />;
  }
}
```

# But what if...?

Nope. If you break any of these rules, React will not work as advertised.

Yes, this is limiting but it also the tradeoff that allows us to do many complex things magically for you in React. Luckily we have patterns to help you avoid needing to break these rules.