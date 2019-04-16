# Custom Elements

#### Autonomous Custom Element
Based on `HTMLElement`

```javascript
// Controller
cont helloElement = customElement('HelloElement', HTMLElement, {
  observedAttributes: [],
 attributeChangedCallback,
 connectedCallback,
 disconnectedCallback,
 adoptedCallback,
 connectedCallback,
});

wave `
_sidebars
<left-sidebar shadow="close">
  <div>Left Sidebar<div>
</left-sidebar>`
```

#### Customized Built-in Element
Based on built-in Elements. E.g. `HTMLButtonElement`

#### Name 
The custom element name. This is defined as the top-level constitiuent tag. 
`<left-menu $leftMenu ></left-menu>`

#### Constructor
The autonomous or customized constructor

#### Observed Attributes 
An array of observed attributes 

#### Lifecycle callbacks
An object of lifecycle methods, element is passes as the first param 

#### Attach Shadow 
```javascript
`<some-element $someElement shadow="open"`
```
$someElement.shoadowRoot
