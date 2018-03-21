Stored Components
===

Keep a React component's state elsewhere for when you'll want it later.

### Installation

```bash
npm install -S stored-component
```

### Usage


#### Basic Usage
Instead of extending from `React.Component` use `StoredComponent` from the
package. In addition, pass the default prop values to `super()`.

```typescript
import * as React from "react";
import StoredComponent from "stored-component";

class MyComponent extends StoredComponent {
  constructor(props) {
    super(props, {foo: "bar", enabled: false});
  }

  // ...
}
```

Because the `StoredComponent` class uses the `componentDidUpdate()` callback to
keep the data store in sync, if your component includes an implementation of
`componentDidUpdate()` you'll need to call `super.componentDidUpdate()`:

```typescript
class MyComponent extends StoredComponent {
  constructor(props) {
    super(props, {foo: "bar", enabled: false});
  }

  componentDidUpdate() {
    console.log("Updated!");
    super.componentDidUpdate();
  }
}
```

#### Per-Instance Storage

By default a `StoredComponent` will associate its stored state with its class
name. Without changing this behavior all `MyComponent`s as defined above would
share the same state. This can be overridden by passing in an `id` parameter to
the `StoredComponent` constructor:

```typescript
class MyComponent extends StoredComponent {
  constructor(props) {
    super(props, {foo: "bar", enabled: false}, props.id);
  }

  // ...
}
```

#### Typing

This project supports TypeScript. You can set the state and prop types of your
`StoredComponent`s just as with React Components:

```typescript
type StateType = typeof defaultState;
const defaultState = {foo: "bar", enabled: false};

class MyComponent extends StoredComponent<{}, StateType> {
  constructor(props) {
    super(props, defaultState);
  }

  // ...
}
```

### Recalling the Original State

You can reset a component's state to its default with the `defaultState()`
method on the `StoredComponent` class.

#### Using a Different Data Store

The default data store used is `window.sessionStorage`. Other storage
implementations can be set using the `setStore()` function.

```typescript
import { storage } from "stored-component";
storage.setStore(window.localStorage);
```

Take note that if you use localStorage you will need to be aware of state model
changes. If your application is updated with different a new state structure,
the old state still sitting in localStorage will be loaded up anyway.
