> ### WAVE-Architecture
Directory Structure
=================

>> The WAVE Architecture is implied upon the top-level source code directory _commonly_ `./src/`.
The structural rules are not intended to depict the entirety of the src directory but to conflate with other modules
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


#### Conventions 
- Multiple markup-templates can be placed in a single view file and exported via named exports. 
- A view file should not import another view file.
- View files should not import logic from controllers.
- Controllers should import markup from view files when plausible.

#### WAVE File Structure
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
 |   |--events.js
 |--routes.js
 |--state-tree.js
 |--config.json
 |--main.js
```
- **background-services**: Operations that run without human intervention. These operations may run in web workers but are no limited to the main thread.
- **local-services**: Allows you to set and access browser storage (local-storage, session-storage, indexeddb, other)
- **web-services**: Methods for using xmlhttprequest. All external data request are made here.
- **helpers**: Helper files and or directories
- **ui**: All visual Ui components. If a component has no view then it shouldn't belong in the ui directory. _[required]_
- **state-tree.js**: A central place for stateObjects to be used globally across the application.
- **config.json**: A human editable config that is fetched into main.js, can also be config.js if feasible.
- **main.js**: The main application file. The root import. _[required]_
- **ui/render.js**: A function or set of functions that renders the initial views depending on when the application loads. _[required]_
- **ui/events.js**: Global events that affect all aspects of the SPA.
- **routes.js**: A list of routes using first-class functions for validation.

##### Nesting [guide]
Avoid deep nesting. Strive to limit the directory depth to a max of 4 levels if possible _this obviously cannot always be attained_. Adopting a prefix can sometimes reduce folder depth complexities:
_3 levels deep_
```bash
/src/ui/page/
             home/
                  controller.js
                  view.js
                  events.js
             about/
                  controller.js
                  view.js
                  events.js
             contact/ 
                  controller.js
                  view.js
                  events.js
```
_2 levels deep_
```bash
/src/ui/
        page-home/
                  controller.js
                  view.js
                  events.js
        page-about/
                  controller.js
                  view.js
                  events.js
        page-contact/ 
                  controller.js
                  view.js
                  events.js
```

Wave Architecture 2019 [MIT](https://github.com/julienetie/wavefront/blob/master/LICENSE)
