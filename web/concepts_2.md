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

# 
