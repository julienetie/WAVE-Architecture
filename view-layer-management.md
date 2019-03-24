> #### WAVE Architecture
View-Layer Management
=========

>>At the heart of the WAVE Architecture is a set of view-layer conventions that take strong advantage of ECMAScript 2015 (6) features. The aim of these requirements are to: 

- Separation of concerns 
- Simplistic selector referencing
- Reduce common DOM based over-egineering complexities
- Improve DOM memeory management


markupEngine that references required selectors and places them into a componentRegistry.

#### Component Registry

The component registry is an object containing namespaces, components and constituents. 
- namespace: A namespaced object literal that is used to group components.
- component element: The topmost element of a component.
- constituents: All elements that fall under the component defined within the same template.

Referencing child-components hierarchically should be avoided and does not serve as an advantage from templating or memory management.

##### Selectors
Selectors are define within a markup template using the dollar-sign symbol.
Namespaces begin with an underscore and are nested by the period symbol.
$component is a reserved word.
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
```bash
selectorRegistry:                        Object
      _messages:                         Object
           _introductory:                Object
                 $welcomeMessage:        WeakMap
                         Object:
                            - $component: Node
                            - $greeting: Node     
                            - $followUp: Node
                            - $content:  Node

componentRegistry:                       Object
      _messages:                         Object
           _introductory:                Object
                 $welcomeMessage:        Node
```
The registry should not be accessible outside of the selector interface. 

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
