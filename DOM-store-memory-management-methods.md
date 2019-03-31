DOM Store Memory Management Methods
=================

This document defines a set of methods for managing DOM elements within a library store.

- selector node: A node or element that may or maynot belong to a document
- selector string: A string that represents an attribute of a node or set of nodes.
- selector reference string: A DLS based string selector


Validate <document-object>
- if <document-object> is undefined <context-object> is window.document. 
- if <document-object> instanceof HTMLDocument is false abort purge.
- Return true.


### delete(<selector>)
When delete is invoked with a selector 
1. If validateDocumentObject is true continue.
2. The corresponding node of the selector is found.
3. All references to that node is removed from all stores maintained by the library.

### purge(<document-object>, <selector>)
1. If validateDocumentObject is true continue.
1. When purge is invoked with a selector   
2. The <document-object> is queried using the given selector.
3. If the node is found it will be removed from the DOM.
4. delete(node) will be performed.

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


