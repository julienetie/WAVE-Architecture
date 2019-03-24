Selectors
=========

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

$.removeChild('$welcomeMessage', '$greeting') // Replaces the weakmap without the child
$.remove('_messages._introductory.$welcomeMessage'); // Removes component from registry (not DOM)

// Removes element and references from the registry 
$.purge('_messages._introductory.$welcomeMessage');
$.purge('$welcomeMessage'); // Remove first found 
$.purge('$welcomeMessage:1'); // Remove 2nd found
$.purge('$welcomeMessage:all'); // Removes all

// set @: If a new template is created it will throw an error if there is a naming conflict 
Unless the namespace is prefixed with an @ sign.

`
