## DOM
The DOM (Document Object Model) in JavaScript is an API (Application Programming Interface) for HTML and XML documents. It represents the structure of a web page as a tree of objects (nodes) that can be manipulated programmatically. This allows JavaScript to interact with the webpage's structure, style, and content dynamically.

### 1. **Document as a Tree Structure**:
The DOM treats an HTML document as a tree structure where each element is a node. The root of this tree is the document object, and all elements, attributes, and text inside the HTML document are represented as nodes in this tree.

### 2. DOM Nodes:
The DOM represents the entire HTML document as a collection of nodes, each of which represents a part of the document.

- **Element Nodes**: Represent HTML elements like <div>, <p>, etc.
- **Text Nodes**: Represent the text inside elements.
- **Attribute Nodes**: Represent the attributes of elements, like id, class, etc.
- **Comment Nodes**: Represent comments inside the HTML.
### 3. Accessing the DOM
```
//To select an element by its id
const heading = document.getElementById('heading');

//To select elements by tag name
const paragraphs = document.getElementsByTagName('p');

```

### 4. Manipulating DOM Elements
```
document.getElementById('heading').innerHTML = "New Heading";
document.getElementById('myImage').src = "newImage.png";



// Create a new element
const newElement = document.createElement('div');
newElement.textContent = "I'm a new div!";

// Append it to the body
document.body.appendChild(newElement);

// Remove an element
const elementToRemove = document.getElementById('oldElement');
elementToRemove.remove();

```

### 5. Event Handling
```
document.getElementById('myButton').addEventListener('click', function() {
  alert('Button was clicked!');
});
```

### 6.Traversing the DOM: You can navigate between nodes (elements) in the DOM tree to reach different parts of the document.

- **Parent Node**: `element.parentNode`
- **Child Nodes**: `element.childNodes`
- **Next Sibling**: `element.nextSibling`
- **Previous Sibling**: `element.previousSibling`
