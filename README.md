Rete context menu plugin
====
#### Rete.js plugin

```js
import ContextMenuPlugin, { Menu, Item, Search } from 'rete-context-menu-plugin';

editor.use(ContextMenuPlugin, {
    searchBar: false, // true by default
    searchKeep: title => true, // leave item when searching, optional. For example, title => ['Refresh'].includes(title)
    delay: 100,
    allocate(component) {
        return ['Submenu'];
    },
    rename(component) {
        return component.name;
    },
    items: {
        'Click me'(){ console.log('Works!') }
    },
    nodeItems: {
        'Click me'(){ console.log('Works for node!') }
    },
    getNodeItems: node => {
        if (node.name === 'Add') {
            return {
                'Only for Add nodes': () => { console.log('Works for add node!') },
            };
        }
    },
    vueComponent: CustomVueComponent // extends Menu
});
```
| Options | Description | Default |
|-|-|-|
| `searchBar` | Showing search bar | `true`
| `delay` | Delay hide, ms | `1000`
| `allocate` | function for placing of components into submenu | `() => []`
| `rename` | function for renaming of items| `component => component.name`
| `items` | custom items (`Object` with nested objects and functions) | `{}`
| `nodeItems` | custom items for Node menu | `{}`
| `getNodeItems` | a function that returns nodeItems for a specific node (arg: Node) | `() => undefined`


You can arbitrarily put a component in a submenu. Examples: 

```js
allocate() { return ["Single submenu"] }
```

```js
allocate(component) { return component.path } // where path is a stack of menu for every component
```


```js
allocate(component) { return null } // exclude component from menu items
```

To change the items that create nodes, you may need to change the name.

```js
class MyComponent {
    constructor() {
        super("My comp");
        this.contextMenuName = "Add My comp";
    }
}
///
rename(component) { return component.contextMenuName || component.name }
```
