
Programming Conventions
=================

- Short-lived node reference: A temporary node reference for a set of one-time operations.
- Long-lived node reference: A node reference that will be reused 

#### Avoid long-lived DOM nodes that have no re-use
If a node is being created with the intention to be attached to the DOM immediately on execution and will not be re-used, it's references 
should die on execution.

_Expensive short lived example_
```javascript
// Top level

let someElement;  // AVOID STORING NODES OUT OF EXECUTION CONTEXT

const createElement = () =>{
  ...
  someElement = ...// New node
}

export default appendElement(someElement);

```

- Do not assign variables directly in the top-level scope to short-lived nodes _[required]_
- Use pure functions when generating short-lived nodes (avoid assigning outer variables to nodes)
- Do not export nodes, export functions that return nodes _[required]_
- Avoid the use of _let_ if mutation is not required
- All code within your WAVE architecture that manages nodes should ideally be wrapped in functions 
- You can assign references to null to enable the node for garbage collection on the next GC sweep _[rare occurrences]_
- nodes that are WeakMap keys will be freed once there are no more references to those nodes. 

#### Use WeakMaps for long-lived DOM nodes. 
Sometimes you want a node tree to remain in memeory, e.g. a modal window.
You can use a WeakMap to reference 
