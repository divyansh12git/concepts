# Error Boundaries
An Error Boundary in React is a special component that handles JavaScript errors during rendering, in lifecycle methods, 
and in constructors of the whole React component tree. By using an Error Boundary, you can catch 
and display an error message or fallback UI instead of crashing the entire application.
1. **Error Boundaries only catch errors in their child components**: They do not catch errors inside themselves, event handlers, asynchronous code (e.g., setTimeout or requestAnimationFrame callbacks), server-side rendering, or errors thrown in the error boundary itself.
2. **Lifecycle Methods for Error Boundaries:**
  - **static getDerivedStateFromError(error)**: This lifecycle method is used to update the state when an error occurs. It receives the error that was thrown, and you can return an updated state to show fallback UI.
  - **componentDidCatch(error, info)**: This method is used for side effects like logging the error or sending the error report to a server. It receives the error and information about where the error occurred.
```
  import React, { Component } from 'react';

class ErrorBoundary extends Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render shows the fallback UI
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // You can log the error to an error reporting service
    console.error("Error caught by Error Boundary:", error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // Render fallback UI
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children; 
  }
}

export default ErrorBoundary;

```
Wrap the components you want to monitor for errors with the ErrorBoundary component.
```
import ErrorBoundary from './ErrorBoundary';
import MyComponent from './MyComponent';

function App() {
  return (
    <ErrorBoundary>
      <MyComponent />
    </ErrorBoundary>
  );
}
```

# React Virtual DOM
The React Virtual DOM (Virtual Document Object Model) is a lightweight copy of the actual DOM. It allows React to efficiently update and render changes in the user interface by keeping track of changes between different states of the DOM without directly manipulating the browser’s real DOM, which is slower.

1. **DOM in a Browser:** The DOM represents the structure of an HTML document as a tree of nodes (elements, attributes, text, etc.). Manipulating the DOM directly can be slow because the browser has to repaint and reflow each time there is a change.
2. **Why Virtual DOM?**

  - Directly updating the real DOM frequently can cause performance bottlenecks, especially in complex UIs with many elements.
  - To address this, React uses the Virtual DOM to optimize how updates are made to the actual DOM. By using a Virtual DOM, React can batch updates and minimize interactions with the real DOM, which is computationally expensive.

## How Virtual DOM Works:
### 1. Virtual DOM Creation:

When you write React components, React creates a virtual representation of the real DOM using JavaScript objects. This is a lightweight version of the actual DOM, known as the Virtual DOM.
### 2. React's Render Method:

- When the state or props of a component change, React will re-render the component, creating a new Virtual DOM tree to represent the updated state.
### 3. Diffing Algorithm:

- React then compares (or diffs) the new Virtual DOM with the previous version of the Virtual DOM. This is done using an efficient algorithm known as **Reconciliation.**
- React finds the differences (changes in elements, attributes, or styles) between the two versions of the Virtual DOM.
### Batch Updates:

- After determining the minimal number of changes needed, React batches these updates and applies them to the real DOM in one go. This minimizes the number of reflows and repaints the browser has to perform, leading to better performance.
### Updating the Real DOM:

- Once the diffing process is complete and the necessary changes are determined, React updates the real DOM with only the changes needed, rather than re-rendering the entire DOM tree.

## Benefits of the Virtual DOM:
### Performance:

- By minimizing direct interactions with the real DOM and batching updates, React significantly improves the performance of applications, especially in cases with frequent UI updates.
### Declarative UI:

- With React, developers describe what the UI should look like in different states, and React takes care of how to update the UI efficiently. The Virtual DOM is a key part of this abstraction.
### Predictability:

- The Virtual DOM ensures that UI updates are handled in a predictable manner. React guarantees that the UI will be updated as per the current state of the Virtual DOM.