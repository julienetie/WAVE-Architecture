# Project Structure

The WAVE Architecture is implied upon the top-level source code directory _commonly_ `src\`.
The structural rules are not intended to depict the entire src directory but to conflate with existing practises.

#### Directory and File Names
- Names should be descriptive avoiding abbreviations and shorthand.
- Ideally lower-case using hyphens to separate words.

#### Component Structure
- **Controller**: The main logical file should be named `controller.js` or `<component-name>.js`. 
There should be one controller in the top level of a component directory. 
The controller is the initial point of import for a component. 

- **Subsequent Controllers**: Other controllers should be names accordingly to their action. No prefix is required.


- **View**: The main markup-template file should be named `view.js` or `<component-name>-view.js`. 
There should be one view in the top level of a component directory. 
The component's view should only be imported by it's controller or a controller of another component. 

- **Subsequent Views**: All other views should be prefixed with the word "view" e.g. `button-view.js`.

- **Events**: Event logic should ideally be separated from a controller. Events should be named `events.js`, 
`<component-name>-events.js` or `<event-type>-events.js` to be used at discretion.


### Conventions 
- There should be no CSS or preprocessor files within the component structure.
- Documentation files are acceptable.
- Multiple markup-templates can be placed in a single view file and exported via named exports. 
- View files should not import other markup-templates from view files.
- View files should not import logic from controllers.
- Controllers should import markup from view files when plausible.
