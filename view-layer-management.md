> #### WAVE Architecture
View-Layer Management
=========

>>At the heart of the WAVE Architecture is a set of view-layer conventions that take strong advantage of ECMAScript 2015 (6) features. The aim of these requirements are to achieve: 

- Separation of concerns 
- Simplified selector referencing
- Reduced real-world complexites for UI development
- Improved DOM manipulation memeory management

WAVE requries a view-layer engine to generate DOM components via markup-templates using [Template literals (Template strings)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals). The engine is responsible for:

- Interpreting template literals
- Registering template namespaces
- Registering and naming the component
- Registering and naming constituents
- Accessing component constituents
- Removing components
- Removing component constituents




##### Declarative Template definitons: 
- **component-element**: The topmost element of a component.
- **constituents**: All selector-elements. A component-element is also a constituent. 
- Selectors: Are define within a markup template using the dollar-sign symbol.
- Namespaces: Begin with an underscore and are nested using dot notation.
- $component is a reserved word

##### Node-Registries:
- **Component Registry**: An object containing keys as component names and values as nodes. 
- **Constitiuent Registry**: An object containing nested namespaces as objects and groups of constituents as a WeakMap.  

```html
import markupEngine from 'markup-engine'; // library agnostic 

export default () => markupEngine `_messages._introductory 
<div $welcomeMessage >
  <h1 $greeting >Hello World</h1>
  <h2 $followUp >How are you?</h2>
  <article $content >..Content goes here</article>
</div>
`;
```
The above will be stored in registry using the following convention.
##### Constituent Registry
```javascript
selectorRegistry:                        Object
      _messages:                         Object
           _introductory:                Object
                 $welcomeMessage:        WeakMap
                         Object:
                            - $component: Node
                            - $greeting: Node     
                            - $followUp: Node
                            - $content:  Node
```
##### Component Registry
```javascript
componentRegistry:                       Object
      _messages:                         Object
           _introductory:                Object
                 $welcomeMessage:        Node
```
The registry should not be accessible outside of the selector interface. 

>Referencing child-components hierarchically should be avoided and does not serve as an advantage from templating or memory management.


#### View-Layer Engine Implementation
```javascript
viewLayer `[ markupString ]`
```

The view-layer engine should be a tagged function or standard function that utilizes a tagged function.
- _markupString_
- _**namespace**_: Starts with an underscore. Mulit-level namespaces are delimited using dot-notation.


- _**replace_existing_component**_: Allows the creation of a new component to replace an existing component in the primary registers.
- _**purge_replace_existing_component**_: Removes the specified component from the DOM then performs _replace_existing_component_.
- _**create_namespaces**_: Creates nested objects on primary registers using declaired _namespaces_. Namespaces are optional. The component-name serves as a namespace.  
- _**locate_selectors**_: Detects all valid selectors within element tags. 
- _**mark_selectors**_: Replaces the _dollar-sign_ of all found selectors with _data-selector-_.
- _**createMarkupNode**: Creates the markupNode as HTML, SVG or XML.
- _**removeSelectorAttributes**: Removes the data-selector attribute from a given element.
- _**registerComponent**_
- _**registerConstituents**_
- _**stringArrayToString**_
- _**elementsToFragment**_
- _**markElements**_


Below are minimal requirements to implement a WAVE View-Layer for the web:
1. _markupString_ must be a template literal and have a length greater than 1
2. _namespace_ syntax is optional. A _namespace_ must start with an underscore _(\_)_ followed by the name of the name.
3. _nested_namespace_ can be achived by consecutive _namespace_ declarations separated by dot notation
4. _replace_existing_component_  `@`


##### Selector interface 
```javascript
// Get element 
$.get('_messages._introductory.$welcomeMessage'); // Returns an object containing the $component and it's constitiuents.
$.get('$welcomeMessage'); // Will traverse componentRegistry for the first component reference found.
$.get('$welcomeMessage:1'); // Will traverse componentRegistry for the 2nd component reference.
$.get('$welcomeMessage:all'); // Will traverse componentRegistry for all matching component references.
$.get('$welcomeMessage').thenRemove(); // Removes reference from registry after returning the node, will also remove constituents 

$.deleteChild('$welcomeMessage', '$greeting') // Replaces the weakmap without the child
$.delete('_messages._introductory.$welcomeMessage'); // deletes component from registry (not DOM)

// Removes element and references from the registry 
$.purge('_messages._introductory.$welcomeMessage');
$.purge('$welcomeMessage'); // Remove first found 
$.purge('$welcomeMessage:1'); // Remove 2nd found
$.purge('$welcomeMessage:all'); // Removes all

// set @: If a new template is created it will throw an error if there is a naming conflict 
Unless the namespace is prefixed with an @ sign.
```
##### Nesting Components
The WAVE Architecture aims to simplfy how we interact with the DOM from a practical pespective rather than a
technically capable perspective. Nesting references is not desirable when building componets within the DOM 
as it creates complexiteis that have little to no benefit. 

###### Embedding Elements
```html
export default ({ anHTMLElement }) => markupEngine `_messages._introductory 
<div $welcomeMessage >
  <h1 $greeting >Hello World</h1>
  <h2 $followUp >How are you?</h2>
  <article $content >${ anHTMLElement }</article>
</div>
`;
```
The above element will be apended as part of the final component but will purposely not be referenced
or traversed. The markup-engine will assume that the element was created using WAVE principles therefore 
it should already exist in the registries. 

An array of elements can also be embedded and will be appended in order.

```html
export default ({ arrayOfHTMLElements }) => markupEngine `_messages._introductory 
<div $welcomeMessage >
  <h1 $greeting >Hello World</h1>
  <h2 $followUp >How are you?</h2>
  <article $content >${ arrayOfHTMLElements }</article>
</div>
`;
```
###### Embedding Strings
```html
export default ({ contentComponentStr }) => markupEngine `_messages._introductory 
<div $welcomeMessage >
  <h1 $greeting >Hello World</h1>
  <h2 $followUp >How are you?</h2>
  <article $content >${ contentComponentStr }</article>
</div>
`;
```
`contentComponentStr` is a string that represents part of the component, therefore the defined constituents within the string will be registered under $welcomeMessage. 

An array of strings can also be embeded, each item will be referenced if they contain selector names.

```html
export default ({ itemsStr }) => markupEngine `_messages._introductory 
<div $welcomeMessage >
  <h1 $greeting >Hello World</h1>
  <h2 $followUp >How are you?</h2>
  <article $content >${ itemsStr }</article>
</div>
`;
```
###### Using querySelector and querySelectorAll
The markup-engine elminates the need for querySelector and querySelectorAll for most use-cases but there is no restriction on use of querySelector or querySelectorAll but they should be avoided as an alternative for referencing created nodes.
