### Components:
- Building blocks of React applications, encapsulating UI elements and their logic.
- Can be nested within each other for complex structures.
- Rendered as JSX elements.
### JSX:
- A syntax extension for JavaScript that allows you to write HTML-like structures within JavaScript code.
- Transpiled into regular JavaScript before execution.
### State:
- Data that defines a component's behavior and appearance.
- Managed within components using the useState hook.
- Updates trigger re-rendering of the component.
### Props:
- Data passed down from parent components to child components.
- Immutable and cannot be modified directly within child components.
### React Hooks:
- Functions that let you use state and other React features without writing a class component.

### Role of keys in React:
- Keys are essential for React to efficiently update and re-render lists of elements. They help React identify which elements have changed, been added, or been removed. Without keys, React might not be able to correctly update the DOM, leading to unexpected behavior.

    #### use Cases:
    1. Identification:
        - Each element in a list should have a unique key.
        - The key acts as an identifier for that element, allowing React to track its changes.
    2. Performance Optimization:
        - React uses keys to determine if an element has changed or if a new element needs to be created.
        - By using keys, React can avoid unnecessary re-renders, improving performance.
    3. List ReOrdering:
        - When elements in a list are reordered, React uses keys to determine which elements have moved.
        - This allows React to update the DOM efficiently without creating new elements.
## Fragments:
Fragments are a way to group multiple elements together without introducing an extra element into the DOM. They are useful when you need to render multiple elements within a conditional statement, loop, or as a child of an element that only allows a single child.
```
<React.Fragment>
{/* Multiple elements */}
</React.Fragment>

<>
  {/* Multiple elements */}
</>

 <div>
    {showContent && (
      <>
        <p>This content is shown conditionally.</p>
        <button>Click me</button>
      </>
    )}
  </div>

   <ul>
    {items.map(item => (
      <React.Fragment key={item}>
        <li>{item}</li>
      </React.Fragment>
    ))}
  </ul>
```

### Portals:
Portals provide a way to render child components into a different DOM node than their parent. 
This is useful when you need to render content outside of the current component's DOM hierarchy, such as modals, tooltips, or overlays.
- **DOM Node:** The createPortal function takes two arguments: the child component and the DOM node where it should be rendered.
- **Rendering:** The portal component will be rendered outside of the current component's DOM hierarchy, directly into the specified DOM node.
- **Lifecycle:** The portal component has its own lifecycle and can be mounted, updated, and unmounted independently of its parent.
- **Parent-Child Relationship:** The portal component is still considered a child of its parent component, but it is rendered into a different DOM node.