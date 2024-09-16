## 1. Code Splitting in React
Code Splitting is a powerful optimization technique in React that allows you to split your JavaScript bundle into smaller chunks. This can improve the performance of your application by loading only the code that is needed for a specific part of the application, rather than loading everything upfront.

#### Why Code Splitting is Important
As React applications grow, the size of the JavaScript bundle increases. If **the entire application’s JavaScript is bundled into one large file**, the browser has to load and parse that file all at once, which can lead to:

- **Longer loading times**: Especially for users on slower networks.
- **Unnecessary code execution**: The browser loads and executes code for parts of the application the user may never visit during a session.

Code splitting helps address these issues by loading only the necessary parts of the code when they are needed (on-demand). This improves initial load time and makes the app feel more responsive.

#### At Component level
React supports code splitting at the component level, meaning you can load components and their dependencies dynamically, only when they are required by the user. This is typically done using:

Dynamic `import()` statements in JavaScript.
React’s built-in support through tools like `React.lazy()` and React.Suspense.
```
import React, { Suspense } from 'react';

const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <div>
      <h1>Welcome to My App</h1>
      <Suspense fallback={<div>Loading...</div>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}

export default App;

```
- The LazyComponent will be code split and loaded asynchronously when it’s needed.
- The Suspense component is used to wrap the lazy-loaded component. While the component is being loaded, the fallback prop (e.g., a loading spinner or message) is displayed.
#### At React Router

```
import React, { Suspense } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const Home = React.lazy(() => import('./Home'));
const About = React.lazy(() => import('./About'));
const Contact = React.lazy(() => import('./Contact'));

function App() {
  return (
    <Router>
      <Suspense fallback={<div>Loading...</div>}>
        <Switch>
          <Route exact path="/" component={Home} />
          <Route path="/about" component={About} />
          <Route path="/contact" component={Contact} />
        </Switch>
      </Suspense>
    </Router>
  );
}

export default App;

```

### Benefits of Code Splitting:
#### 1. Improved Performance:

By splitting the code into smaller chunks, the initial load time decreases, which improves performance, especially for larger applications.
#### 2. Faster User Experience:

Code is fetched dynamically only when needed, allowing the user to interact with the app more quickly.
#### 3. Reduced Bandwidth Usage:

Code splitting reduces the amount of JavaScript that needs to be downloaded at once, which can save bandwidth, particularly for mobile users or users with slow connections.
#### 4. Efficient Caching:

Browsers can cache code chunks more effectively. Once a chunk is loaded, it remains in the cache until the code changes, reducing future loading times.

#### WebPack
Webpack: React applications created with create-react-app or custom setups using Webpack automatically support code splitting via dynamic import() calls.

#### Common Pitfalls
- **Too Many Small Chunks:** If overused, code splitting can create too many small chunks, which can cause a performance hit due to too many network requests. Balance is essential.

- **Fallback Management**: It’s important to handle fallbacks in the Suspense component properly, especially when code splitting is used with routing. If the user navigates too quickly between pages, loading indicators can become too frequent.

- **SEO Concerns**: Code splitting on the client side does not help with SEO for initial page loads. Server-side rendering (SSR) combined with code splitting can help mitigate this issue.



## 2. Error Boundaries
An Error Boundary in React is a special component that handles JavaScript errors during rendering, in lifecycle methods, 
and in constructors of the whole React component tree. By using an Error Boundary, you can catch 
and display an error message or fallback UI instead of crashing the entire application.

Error boundaries are React components that catch JavaScript errors anywhere in their **child component** tree, **log those errors, and display a fallback UI** instead of the component tree that crashed. 

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

## 3. Higher Order Components(HOCs) 
A Higher-Order Component (HOC) is an advanced pattern in React used for reusing component logic. It is a function that takes a component as an argument and returns a new component with enhanced functionality.

Think of HOCs as a **wrapper or a decorator** around a component to extend its behavior, add features, or share logic between components without modifying them directly.

```
const higherOrderComponent = (WrappedComponent) => {
  return class extends React.Component {
    render() {
      return <WrappedComponent {...this.props} />;
    }
  };
};
```
#### Use Case:
1. **Reusability of Logic**: HOCs allow you to encapsulate common logic that can be shared across multiple components. For example, adding authentication checks, logging, or data fetching.

2. **Cross-Cutting Concerns**: Common concerns that affect many parts of the application can be abstracted into HOCs, such as handling permissions, injecting themes, or handling error boundaries.

3. **Code Organization**: HOCs help you keep your component code clean by moving shared behaviors into separate, reusable functions.

 #### Authentication Check with HOC
 ```
 const withAuth = (WrappedComponent) => {
  return class extends React.Component {
    componentDidMount() {
      if (!this.props.isAuthenticated) {
        // Redirect to login or show a message
        window.location.href = '/login';
      }
    }

    render() {
      if (this.props.isAuthenticated) {
        return <WrappedComponent {...this.props} />;
      } else {
        return null; // or a loading spinner
      }
    }
  };
};

// Usage
const Dashboard = (props) => {
  return <div>Welcome to the Dashboard, {props.user}</div>;
};

const ProtectedDashboard = withAuth(Dashboard);

 ```
####  Alternatives to HOCs
With the introduction of React Hooks, many use cases for HOCs can now be handled using hooks, which are often simpler and more readable. For example, instead of using an HOC for data fetching, you can use the useEffect hook in a functional component.

#### Common Pitfalls of HOCs
- **Props Collisions**: Be careful of name collisions between props in the HOC and the wrapped component. Make sure that the HOC doesn’t overwrite existing props passed to the wrapped component.

- **Static Methods and Properties:** HOCs don’t automatically copy static methods from the original component. You need to manually copy static properties (using a utility like hoist-non-react-statics).

- **Render Chain Complexity**: Using too many HOCs can make the render chain complex and harder to debug. 

## 4. Props Injection in React
Props Injection is a pattern used in React to dynamically inject props into a component, often to provide additional data or functionality without directly modifying the component itself. This technique is particularly useful when dealing with reusable components, higher-order components (HOCs), or context.

Props injection can be seen as a way to pass additional props to a child component, enabling greater flexibility and reuse of components across an application.

### Use Cases for Props Injection
#### Enhancing Components with HOCs:

**Higher-Order Components (HOCs)** often use props injection to pass additional props like state or handler functions to the wrapped component.
Context API:

**The React Context API** can be used to provide props from a higher level in the component tree to any deeply nested component without passing props manually at every level.
**Dynamic Customization**  When you need to dynamically inject behavior, props injection allows you to modify a component's behavior or data without altering its internal logic.

#### Props Injection with HOC
```
// Higher-Order Component that injects props

function withAdditionalProps(WrappedComponent) {
  return function EnhancedComponent(props) {
    // Injecting additional props
    const injectedProps = { extraData: 'Injected Prop!' };

    return <WrappedComponent {...props} {...injectedProps} />;
  };
}
const EnhancedComponent = withAdditionalProps(MyComponent);
```

#### Props Injection with Context API

Another common way of injecting props is via the Context API, which allows you to provide data deep in the component tree without passing props manually at every level.

```
import React, { createContext, useContext } from 'react';

// Create a Context
const ThemeContext = createContext();

// Component that consumes the injected prop
function ThemedComponent() {
  const theme = useContext(ThemeContext);
  return <div style={{ color: theme }}>This is a {theme} theme!</div>;
}

// Parent component that provides the prop
function App() {
  return (
    <ThemeContext.Provider value="dark">
      <ThemedComponent />
    </ThemeContext.Provider>
  );
}

export default App;

```

#### Advantages of Props Injection
1. Component Reusability:
2. Separation of Concerns:
3. **Dynamic Behavior:** Components can receive dynamic behavior or configuration based on context or higher-level components.

#### Potential Pitfalls
1. Over-Injecting Props:
2. Props Clashing: If injected props conflict with existing props, this can lead to unintended behavior. Careful naming conventions can help prevent this.


## 5. Render Props in React

Render Props is a React pattern used for sharing logic between components. In this pattern, **a prop is passed to a component, which is a function** that returns a React element. 
This function allows the **parent component** to determine what content will be rendered by the **child component**.

Render Props are useful for reusing component logic without the need for higher-order components (HOCs) or inheritance.

#### Why Use Render Props?
In scenarios where you need to share stateful logic between components (like fetching data, tracking state, or managing events), the render props pattern enables this in a declarative way, keeping the logic in a shared component and allowing the UI to be customized by a function passed through props.

in class components:
```
// Example: A component that uses a render prop
class MouseTracker extends React.Component {
  state = { x: 0, y: 0 };

  handleMouseMove = (event) => {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  };

  render() {
    return (
      <div onMouseMove={this.handleMouseMove}>
        {/*
          Instead of rendering the same thing all the time,
          MouseTracker uses the render prop to dynamically decide what to display
        */}
        {this.props.render(this.state)}
      </div>
    );
  }
}

// Usage of MouseTracker
function App() {
  return (
    <MouseTracker
      render={({ x, y }) => (
        <h1>
          The mouse position is ({x}, {y})
        </h1>
      )}
    />
  );
}

export default App;

```
Through React Hooks:

```
import { useState, useEffect } from 'react';

function useMousePosition() {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  useEffect(() => {
    const handleMouseMove = (event) => {
      setPosition({
        x: event.clientX,
        y: event.clientY,
      });
    };
    window.addEventListener('mousemove', handleMouseMove);
    
    return () => window.removeEventListener('mousemove', handleMouseMove);
  }, []);

  return position;
}

function App() {
  const { x, y } = useMousePosition();
  
  return <h1>The mouse position is ({x}, {y})</h1>;
}

export default App;

```

#### Potential Pitfalls
**1. Extra Re-Renders:**

Using inline functions for render props can cause unnecessary re-renders, as new function instances are created on every render. You can optimize this by moving the render function outside of the render method.
**2. Nested Render Props:**

If multiple components rely on render props, they can lead to deeply nested JSX. This problem is similar to the "callback hell" issue in JavaScript, though it can be managed with hooks or careful component design.

## 6. Portals:
Portals provide a way to render child components into a different DOM node than their parent. 
This is useful when you need to render content outside of the current component's DOM hierarchy, such as modals, tooltips, or overlays.
- **DOM Node:** The createPortal function takes two arguments: the child component and the DOM node where it should be rendered.
- **Rendering:** The portal component will be rendered outside of the current component's DOM hierarchy, directly into the specified DOM node.
- **Lifecycle:** The portal component has its own lifecycle and can be mounted, updated, and unmounted independently of its parent.
- **Parent-Child Relationship:** The portal component is still considered a child of its parent component, but it is rendered into a different DOM node.
```
import React from 'react';
import ReactDOM from 'react-dom';

// Modal component that uses a portal
function Modal({ children, onClose }) {
  return ReactDOM.createPortal(
    <div style={modalStyles}>
      <button onClick={onClose}>Close</button>
      {children}
    </div>,
    document.body // Render the modal into the body element
  );
}


// Main App component
function App() {
  const [isModalOpen, setIsModalOpen] = React.useState(false);

  return (
    <div>
      <button onClick={() => setIsModalOpen(true)}>Open Modal</button>
      {isModalOpen && (
        <Modal onClose={() => setIsModalOpen(false)}>
          <h1>Modal Content</h1>
        </Modal>
      )}
    </div>
  );
}

export default App;

```
 #### Explanation
**ReactDOM.createPortal**: Renders the Modal component into document.body, which is outside the normal DOM hierarchy of the App component.
modalStyles: Styles for the modal to ensure it appears in the center of the screen.
### Benefits of Using Portals
#### Z-Index Management:
Portals allow you to render components that need to be on top of others, such as modals or tooltips, without worrying about the z-index stacking within their parent component.
#### Avoid Overflow Issues:

Portals can be used to render elements like modals or dropdowns that need to escape overflow constraints of their parent containers.
#### Separation of Concerns:
By rendering a component into a different part of the DOM, you can keep the component's logic separate from its visual placement.
