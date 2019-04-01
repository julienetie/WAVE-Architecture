Node Lifecycle Methods
=================

This document defines a set of methods for managing DOM elements within a library store.

- selector node: A node or element that may or maynot belong to a document
- selector string: A string that represents an attribute of a node or set of nodes.
- selector reference string: A DLS based string selector


Validate <document-object>
- if <document-object> is undefined <context-object> is window.document. 
- if <document-object> instanceof HTMLDocument is false abort purge.
- Return true.
  
  
LookupNode
1. TBA
  
<selector>: [node](https://developer.mozilla.org/en-US/docs/Web/API/Node) | [CSS-selector](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Selectors) | library-specific-selector


### delete(<selector>)
- Lookup datasets referencing the given node using the selector
- If no reference is found aboart delete
- Else remove the node's reference from aforementioned datasets
 

### purge(_document-object_, _selector_)
- If document-object is undefined document-object is window.document
- The selector is queried against the document-object to return the queriedNode
- if queriedNode is null 
- - Invole delete(_node_)
- Else if queryiedNode is a node and is owned by the document-object 
  - Invoke Node.remove()
- - Invoke delete(_node_)
  

### clense(<document-object>, <selector>)
1. If validateDocumentObject is true continue.
2. When clense is invoked with a selector  
3. The corresponding node of the selector is found.
4. If the node is connected to the <document-object>, abort clense.
5. If the node is disconnected from the <document-object> delete(node) will be performed.

### rehydrate(<document-object>, [<selector>, <replacement-selector])
1. If validateDocumentObject is true
2. When rehydrate is invoked with a selector and replacement-selector.
3. The corresponding nodes of the selector is found
4. If the node is connected to the <document-object>, abort rehydrate.
5. If the node is disconnected from the <document-object> replace the node with <replacement-selector>


Invoke lookupNode with parameter _selector_ 
