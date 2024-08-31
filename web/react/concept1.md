## 1. Lifetime Method (Class Component):
React lifecycle methods are functions that are called at different stages of a component's life. They are essential for managing side effects, updating state, and performing cleanup tasks.
- ### Mounting
   - **constructor:**
      -  Called when the component is first created.
      -  Initializes the component's state and props.
      - Typically used to set initial state values.
    - **componentDidMount:**
        - Called after the component is rendered to the DOM.
        - Used for tasks like fetching data, setting up subscriptions, or updating the DOM directly.
        - A common use case is to fetch data from an API and update the component's state.
- ### Updating:
    - **shouldComponentUpdate:**
        - Called before the component re-renders.
        - Determines whether the component should re-render based on changes to its props or state.
        - By default, it returns true, causing the component to re-render.
    - **componentWillUpdate:**
        - Called before the component re-renders.
        - Used for tasks that need to be performed before the component's state or props are updated.
        - However, it's generally recommended to use useEffect for most side effects in functional components.
    - **componentDidUpdate:**
        - Called after the component re-renders.
        - Used for tasks that need to be performed after the component's state or props have been updated.
        - Common use cases include updating the DOM directly, performing calculations, or fetching data based on the updated state.
- ## Unmounting:
    - **componentWillUnmount:**
        - Called before the component is removed from the DOM.
        - Used for cleanup tasks such as canceling timers, subscriptions, or removing event listeners.
        - This is essential to prevent memory leaks.

#### In functional components, lifecycle methods are replaced by hooks. The most commonly used hook for managing side effects is useEffect.
## useEffect:
- Can be used to simulate lifecycle methods in functional components.
- Takes two arguments: a callback function and an optional dependency - array.
- The callback function is executed after the component is rendered to the DOM and whenever the dependencies in the array change.
- By using the dependency array, you can control when the effect is run.
- To simulate componentDidMount and componentDidUpdate, include the effect without dependencies.
- To simulate componentWillUnmount, use the cleanup function returned by useEffect.