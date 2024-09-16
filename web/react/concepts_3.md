## React Fiber Architecture

React Fiber is the reimplementation of React’s core algorithm and rendering engine, introduced in React 16. It was designed to address several limitations of the original React (React Stack) and improve how React handles rendering, prioritization, and updates.


#### Why React Fiber Was Introduced
Before Fiber, React used a synchronous rendering engine, known as the "Stack Reconciler", which would process the entire component tree in one go. This approach had limitations:

- **Synchronous Updates**: Once rendering started, React had to complete it before handling other tasks. This could lead to a poor user experience (UX) in complex apps.
- **No Prioritization**: All rendering tasks were treated equally. For instance, urgent updates like animations or user input had to wait for other lower-priority updates, leading to potential frame drops or UI jank.
- **Blocking Rendering**: Long-running rendering tasks could block the main thread, making the UI unresponsive.

React Fiber was designed to solve these problems by breaking rendering into **incremental steps**, allowing React to **pause and prioritize work**.

### What is React Fiber?
React Fiber is a reconciliation algorithm that enables React to split the rendering work into small units, called fibers, and spread it out over multiple frames. This allows React to be asynchronous and more responsive.

Fiber introduces two key concepts:

1. **Incremental Rendering**: React can pause, abort, or continue rendering work based on priorities, making the rendering process non-blocking.
2. **Prioritization of Updates**: Different updates can be assigned different priorities, allowing more urgent updates (like user input) to be processed before less critical ones (like network data updates).

### Key Features of React Fiber
#### 1. Concurrency and Scheduling:
- React Fiber allows React to schedule work and prioritize updates. This is done through the scheduler within React, which can break rendering work into chunks and distribute them over multiple frames.
- The rendering process can be paused, resumed, or aborted, depending on the priority of the task (e.g., animations and user input take higher priority than background data fetching).
#### 2. Time-Slicing:

- Fiber splits rendering work into small chunks that can be processed in idle frames, preventing the UI from becoming unresponsive. This approach is called time-slicing.
- With time-slicing, React can yield control back to the browser after processing a portion of the work, allowing higher-priority tasks to run (like animations, input handling).
#### 3. Interruptible Rendering:

- React Fiber allows rendering work to be interrupted and resumed later, improving performance for tasks that may take a longer time (e.g., rendering large lists).
- React will split the work into smaller units and pause between each unit, ensuring that the browser stays responsive.

#### 4. Error Boundaries:

- React Fiber introduced error boundaries, which allow React components to catch errors in the rendering process and prevent the entire app from crashing.
- This is done by adding a new lifecycle method called componentDidCatch to gracefully handle errors.

#### 5. Suspense:

- React Fiber enables features like Suspense, which allows components to “wait” for asynchronous data to load before they are displayed.
- Suspense is particularly useful in server-side rendering and data fetching scenarios, enabling smoother UX.

#### 6. Backwards Compatibility:

- React Fiber is fully compatible with older React code and the React Stack Reconciler. No changes were needed for most React apps during the transition to Fiber.

### Fiber Data Structure
In React Fiber, each node in the component tree is represented by a Fiber node. These nodes represent the work unit of a particular component and store information about the component and its state.

Each Fiber node contains:

- **Type of the component** (e.g., functional or class component, host component like div, etc.).
- **Props and state** associated with the component.
- **Effects** (side-effects like DOM updates, refs, etc.).
- **Child, sibling, and parent references** (for traversing the tree).

### Phases of React Fiber’s Reconciliation Process
The reconciliation process in React Fiber is divided into two main phases:

#### 1. Render Phase (Reconciliation):

- In this phase, React calculates what changes need to be made to the UI, creating work units that represent updates for each component.
- The render phase is non-committal and can be paused, resumed, or restarted.
- React breaks down the component tree into smaller units of work called fibers. It processes them one at a time, allowing high-priority tasks to interrupt the process.
- Output: This phase produces the list of changes (diffs) that need to be applied to the DOM.
#### 2. Commit Phase:

- The commit phase is where the actual DOM updates take place. This phase is synchronous and cannot be interrupted.
- Once React knows what changes to make, it applies them in this phase (e.g., updating the DOM, running side-effects like useEffect, calling refs).
- This phase is divided into three steps:
    1. Before Mutation: React prepares the DOM for updates.
    2. Mutation: React makes the actual changes to the DOM.
    3. Layout Effects: After the DOM updates, layout effects are executed (e.g., componentDidMount and useLayoutEffect).

### React Fiber's Benefits
1. Smoother User Experience
2. Improved Performance
3. Concurrent Mode (Experimental)
4. Better Error Handling
