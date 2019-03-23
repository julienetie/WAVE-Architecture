# Project Structure

The WAVE Architecture is implied upon the top-level source code directory _commonly_ `src\`.
The structural rules are not intended to depict the entire src directory but to conflate with other modules
required for the application.

#### Directory and File Names
- Names should be descriptive avoiding abbreviations and shorthand.
- Ideally lower-case using hyphens to separate words.

#### Component Structure
- **Controller**: The main logical file should be named `controller.js` or `<component-name>.js`. 
There should be one controller in the top level of every component directory.
The controller is the initial point of import for a component. 

- **Subsequent Controllers**: Other controllers should be names accordingly to their action and do not require a suffix.


- **View**: The main component markup-template file should be named `view.js` or `<component-name>-view.js`. 
There should be one view in the top level of a component directory. 
The component's views should only be imported by it's controller or a controller of another component. 

- **Subsequent Views**: All other views should be prefixed with the word "view" e.g. `button-view.js`.

- **Events**: Event logic should ideally be separated from a controller. Events should be named `events.js`, 
`<component-name>-events.js` or `<event-type>-events.js` to be used at discretion.


### Conventions 
- Multiple markup-templates can be placed in a single view file and exported via named exports. 
- A view file should not import another view file.
- View files should not import logic from controllers.
- Controllers should import markup from view files when plausible.

### WAVE File Structure
Here's an example of the WAVE file structure.  
```bash
src/
 |--helpers/
 |--web-services/
 |--local-services/
 |--background-services/
 |--ui/ 
 |   |--global/
 |   |   |--search/
 |   |   |   |--controller.js
 |   |   |   |--view.js
 |   |   |   |--options.js
 |   |   |   |--options-view.js
 |   |   |   |--events.js
 |   |--footer/  
 |   |   |--controller.js  
 |   |   |--view.js  
 |   |   |--item-view.js
 |   |--side-bar/
 |   |   |--controller.js  
 |   |   |--view.js
 |   |   |--item-view.js
 |   |   |--square-ad-view.js
 |   |   |--events.js
 |   |--header/  
 |   |   |--controller.js
 |   |   |--editor-details-view.js
 |   |   |--view.js
 |   |--render.js
 |--state-tree.js
 |--config.json
 |--main.js
```








