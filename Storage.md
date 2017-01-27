This guide will describe how FullScreenShenanigans projects keep track of game information.

# ItemsHoldr

The ItemsHoldr module is a wrapper around localStorage.
It lets GameStartr instances store data locally with default values and value-based event handlers.

## Local Storage

Other websites also use `localStorage`, so ItemsHoldr has a prefix property to differentiate its information.

```javascript
const itemsHolder = new ItemsHoldr({
    prefix: "FullScreenShenanigans"
});
```

## Items

To add, update, and remove an item in the collection, use `addItem`, `setItem`, and `removeItem` respectively.

```javascript
itemsHolder.addItem("color", { value: "red" });
itemsHolder.setItem("color", "purple");
itemsHolder.removeItem("color");
```

`increase` and `decrease` add and subtract from the item's value.
`toggle` flips the boolean value of the item.

```javascript
itemsHolder.increase("weight", 14);
itemsHolder.decrease("weight", 10);
itemsHolder.toggle("started");
```

`checkExistence` can be used to check if there is an item under the specified key and if not, add it to the collection.

```javascript
itemsHolder.checkExistence("color");
```

By default if an item does not exist when `setItem`, `getItem`, `increase`, or `toggle` are called, the item is then added to the collection.
Define `allowNewItems` to explicitly enable or disable this feature.

```javascript
const itemsHolder = new ItemsHoldr({
    allowNewItems: false
});
```

To manually update a changed item's value in localStorage, use `saveItem`.
To save all the items' values to localStorage, use `saveAll`.

```javascript
itemsHolder.saveItem("color");
itemsHolder.saveAll();
```

### Auto Save

Auto saving, which updates an item's value in localStorage when the value is changed, can be enabled when making the ItemsHolder container.

```javascript
const itemsHolder = new ItemsHoldr({
    autoSave: true
});
```

By default, this is disabled.
To toggle autoSave, use `toggleAutoSave`.

```javascript
itemsHolder.toggleAutoSave();
```

## Updating

Triggers are functions that get run when an item's value equals the specified value.

```javascript
itemsHolder.addItem("color", {
    value: "cyan",
    triggers: {
        red: () => { this.value = "black"; }
    }
});
itemsHolder.setItem("color", "red");
```

Modularity restricts an item's value to a range from 0 to the specified max.
When the value is changed, the `onModular` function is run `x / modularity` times, where `x` is the number the value was set to.
Each time the function is run, value gets set to `value % modularity`.

```javascript
itemsHolder.addItem("counter", { value: 0 });
itemsHolder.addItem("time", {
    value: 0,
    modularity: 12,
    onModular: () => { ItemsHolder.increase("counter", 1); }
});
itemsHolder.setItem("time", 100);
```

## Clearing and Defaults

To clear all items from the collection, use `clear`.

```javascript
itemsHolder.clear();
```

To clear the collection, but keep some items which will always be in the collection, use `valueDefault`.

```javascript
const itemsHolder = new ItemsHoldr({
    values: {
        color: {
            valueDefault: "red"
        }
    }
});
ItemsHolder.clear(); // "color" is still in the collection with a reset value of "red"
```

## Elements

*Note:* This part of ItemsHoldr will be placed into another module in the future.

To signal if a container should be made to hold HTML elements, assign a value to `doMakeContainer`.

```javascript
const itemsHolder = new ItemsHoldr({
    doMakeContainer: true
});
```

Changes to element values will show on the page if the container is appended to an element on the page.
To signal if an item is an element, assign `hasElement` to true.

```javascript
const itemsHolder = new ItemsHoldr({
    doMakeContainer: true,
    values: {
        color: {
            valueDefault: "red",
            hasElement: true
        }
    }
});
```

### Display Changes

If you want certain values to be represented differently on the page, use `displayChanges`.
These changes are hardcoded values that replace specified values when element items are updated.

```javascript
const itemsHolder = new ItemsHoldr({
    displayChanges: {
        Infinity: "INF";
    }
});

itemsHolder.addItem("limit", {
    value: "4000",
    hasElement: true
});
itemsHolder.setItem("limit", "Infinity");
```

`hasDisplayChange` checks to see if a value has a recorded change and `getDisplayChange` returns the entry to replace the value with.

```javascript
itemsHolder.hasDisplayChange("Infinity");
const newValue: string = ItemsHolder.getDisplayChange("Infinity");
```

### Container Visibility

`hideContainer` hides the container from view and `displayContainer` makes the container visible.

```javascript
itemsHolder.hideContainer();
itemsHolder.displayContainer();
```

More can be read about ItemsHoldr on its [Readme](https://github.com/FullScreenShenanigans/ItemsHoldr/blob/master/README.md).


# StateHoldr

StateHoldr is a module for tracking changes of items in ItemsHolder in objects called collections.
Changes are key-value pairs of attributes for items and values.
A collection describes how named items in a group have been changed. For each item in the group, the collection will store a key-value pair of attributes and new values.
For example, a garage collection that contains a "car" item with "color" and "name" changes could be described as:

```javascript
const garage: { [i:string]: { [i: string]: string } } = {
    car: {
        color: "red"
    }
};
```

Collections allow for various attributes for a single item to be grouped together versus storing individual items.

## Prefix

Like ItemsHoldr, StateHoldr has its own prefix property.
All collections in StateHoldr are stored in ItemsHoldr prepended with the StateHoldr prefix.

```javascript
const stateHolder = new StateHoldr({
    ItemsHolder: new ItemsHoldr(),
    prefix: "StateHolder"
});
```

## Collections

The collections the module has recorded are stored in a list keyed by `stateCollectionKeys` in ItemsHolder (no StateHoldr prefix prepended).
StateHoldr always has one collection as its current collection.

`setCollection` sets the current collection.

```javascript
stateHolder.setCollection("newCollection", {
    car: {
        color: "red"
    }
});
```

If the name passed in is the name of a collection that already exists, that collection's old values will be used.

```javascript
stateHolder.setCollection("newCollection", {
    car: {
        color: "red"
    }
});

stateHolder.setCollection("secondCollection");
stateHolder.setCollection("newCollection"); // car is still in this collection
```

Collections aren't put into ItemsHolder until they are saved.
`saveCollection` saves the current collection to ItemsHolder.

```javascript
stateHolder.saveCollection();
```

## Changes

To add a change to the current collection, use `addChange`.
`addChange` takes in the key of the item, an attribute of the item, and the value being stored.

```javascript
stateHolder.addChange("car", "color", "red");
```

A change to a specific collection can be added with `addCollectionChange`.

```javascript
stateHolder.addCollectionChange("otherCollection", "car", "color", "red");
```

To copy changes from an item in the current collection into a target object, use `applyChanges`.

```javascript
const recipient: { [i: string]: string } = {};
stateHolder.applyChanges("car", recipient);
```

`getChanges` returns the changes for a specific item.

```javascript
const changes: { [i: string]: string } = StateHolder.getChanges("car");
```

More can be read about StateHoldr on its [Readme](https://github.com/FullScreenShenanigans/StateHoldr/blob/master/README.md).
