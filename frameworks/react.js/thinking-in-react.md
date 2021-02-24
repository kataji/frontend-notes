# Thinking in React

[https://reactjs.org/docs/thinking-in-react.html](https://reactjs.org/docs/thinking-in-react.html)

### Step 1: Break The UI Into A Component Hierarchy <a id="step-1-break-the-ui-into-a-component-hierarchy"></a>

> But how do you know what should be its own component? Use the same techniques for deciding if you should create a new function or object. One such technique is the [single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle), that is, a component should ideally only do one thing.

### Step 2: Build A Static Version in React <a id="step-2-build-a-static-version-in-react"></a>

> It’s best to decouple these processes because building a static version requires a lot of typing and no thinking, and adding interactivity requires a lot of thinking and not a lot of typing.
>
> If you’re familiar with the concept of _state_, **don’t use state at all** to build this static version. State is reserved only for interactivity, that is, data that changes over time. Since this is a static version of the app, you don’t need it.

### Step 3: Identify The Minimal \(but complete\) Representation Of UI State <a id="step-3-identify-the-minimal-but-complete-representation-of-ui-state"></a>

> The key here is [DRY: _Don’t Repeat Yourself_](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself). Figure out the absolute minimal representation of the state your application needs and compute everything else you need on-demand.

### Step 4: Identify Where Your State Should Live <a id="step-4-identify-where-your-state-should-live"></a>

In short: lowest common ancestor

> For each piece of state in your application:
>
> * Identify every component that renders something based on that state.
> * Find a common owner component \(a single component above all the components that need the state in the hierarchy\).
> * Either the common owner or another component higher up in the hierarchy should own the state.
> * If you can’t find a component where it makes sense to own the state, create a new component solely for holding the state and add it somewhere in the hierarchy above the common owner component.

### Step 5: Add Inverse Data Flow <a id="step-5-add-inverse-data-flow"></a>

For user inputs to affect the states. 

Write state-changing functions in the component that owns the state and pass the function along with the state as props down the component hierarchy. Bind the function to some event handler in the component that receives user input.

