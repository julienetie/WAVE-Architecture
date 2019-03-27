DOM Interface 
=============

> The WAVE Architecture is not intended to create polished interfaces to replace the native Browser API. 
It aims to only tackle areas in a UI workflow that can benefit from abstractions. There are many DOM APIs 
that can be used with the WAVE Architecture, it must be stressed to only use what is necessary. 

#### Attatch 
- append()
#### Replace 
- replaceWith()
#### Detatch
- remove()





##### Conditions & comparisions  
- hasChildNodes()
- contains()
- isConnected
- isEqualNode()
- isSameNode()
>- isDefaultNamespace()
>- compareDocumentPosition()

##### Getters 
- getRootNode()
- nodeType
- baseURI
- querySelector()
- querySelectorAll()
>- nodeName
>- lookupNamespaceURI()
>- lookupPrefix()
>- baseURIObject()

##### Getters & setters 
- textContent
- outerText
>- nodeValue

##### Clone 
- cloneNode()

##### Relationships 
- childNodes
- firstChild
- lastChild
- nextSibling
- ownerDocument
- parentElement
- parentNode
- previousSibling
- childElementCount
- children
- firstElementChild
- lastElementChild

##### Modifers 
- removeChild()
- remove()
- replaceChild()
- replaceWith()
- normalize()
- insertBefore()
- appendChild()
- after()
- before()
- append()
- prepend()
- insertAdjacentElement()
- insertAdjacentText(where, data)
