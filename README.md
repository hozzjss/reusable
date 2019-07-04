[![Build Status](https://circleci.com/gh/reusablejs/reusable.svg?style=svg)](https://circleci.com/gh/reusablejs/reusable)
[![npm version](https://badge.fury.io/js/reusable.svg)](https://badge.fury.io/js/reusable)

# What?
Reusable is a solution for state management, based on React Hooks.

It allows you to transform your custom hooks to singleton stores, and subscribe to them directly from any component.

The benefits:  
- Stores are decoupled from component tree
- You can use selectors and bail out of render
- Stores can co-depend without worrying about provider order
- It's easy to create packages that rely on global state, and plug them into apps seamlessly

# Why do we need (yet) another state management library?
Most current state management solutions don't let you manage state using hooks, which causes you to manage local and global state differently, and have a costly transition between the two.

Reusable solves this by seemingly transforming your custom hooks into global stores.

# Global Stores?!
Sounds like an anti-pattern, but in fact decoupling your state management solution from your component tree gives the developers a lot of flexibilty while designing both the component tree, and the app's state.

# What about hooks+Context?
Using plain context is not a best solution for state management, that led us to write this library:
- When managing global state using Context in a large app, you will probably have many small, single-purpose providers. Soon enough you'll find a Provider wrapper hell.
- When you order the providers vertically, you can’t dynamically choose to depend on each other without changing the order, which might break things.
- Context doesn't support selectors, render bailout, or debouncing

# Check out the video!
Check out the video where we announced the library for the first time and explained all about the problems it aims to solve and how it works:  
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/oy-6urveWzo/0.jpg)](https://www.youtube.com/watch?v=oy-6urveWzo)

# Show me the code!
Wrap your app with a provider:
```javascript
const App = () => (
  <ReusableProvider> {/* no initialization code, stores automatically plug into the top provider */}
    ...
  </ReusableProvider>
)
```

And just pass your custom hooks to `createStore`:

```javascript
const useCounter = createStore(() => {
  const [counter, setCounter] = useState(0);
  
  return {
    counter,
    increment: () => setCounter(prev => prev + 1)
  }
});
```

Then you get a singleton store, and a hook that subscribes directly to that store:
```javascript
const Comp1 = () => {
  const something = useCounter();
}

const Comp2 = () => {
  const something = useCounter(); // same something
}
```

## Selectors
You can also use selectors, and your component will re-render only if the return value changed:  

```javascript
const Comp1 = () => {
  const isPositive = useCounter(state => state.counter > 0);
   // Will only re-render if switching between positive and negative
}
```

## Using stores from other stores
Every store can use any other store, without worrying about provider order.
Just use the store's hook inside the other store:
```javascript
const useCurrentUser = createStore(() => ...);
const usePosts = createStore(() => ...);

const useCurrentUserPosts = createStore(() => {
  const currentUser = useCurrentUser();
  const posts = useCurrentUser();
  
  return ...
});
```

# Docs
Check out the full docs here:
[https://reusablejs.github.io/reusable](https://reusablejs.github.io/reusable)


## Feedback / Contributing:
We would love your feedback / suggestions
Please open an issue for discussion before submitting a PR
Thanks
