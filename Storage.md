This guide will describe how FullScreenShenanigans projects keep track of game information.

### Table of Contents
1. [ItemsHoldr](#itemsholdr)
    1. [Local Storage](#local-storage)
    2. [Items](#items)
    3. [Updating](#updating)
    4. [Clearing and Defaults](#clearing-and-defaults)
    5. [Elements](#elements)
2. [StateHoldr](#stateholdr)

# ItemsHoldr

ItemsHoldr is a versatile container to store and manipulate values in localStorage, and optionally keep an updated HTML container showing these values.

## Local Storage

`localStorage` is a browser property where information is saved without expiring.
ItemsHoldr keeps track of items and their values while the game is running.

Other sources (e.g. websites) also use `localStorage`, so ItemsHoldr has a prefix property to differentiate which information belongs to which.

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({
    prefix: "FullScreenShenanigans"
});
```

## Items

To add an item to the collection, use `addItem` and to remove an item, use `removeItem`.

```typescript
ItemsHolder.addItem("color", { value: "red" });
ItemsHolder.removeItem("color");
```

`setItem` is used to set the value of an item.

```typescript
ItemsHolder.setItem("color", "purple");
```

To manually update a changed item's value in localStorage, use `saveItem`.
To save all the items' values in localStorage, use `saveAll`

```typescript
ItemsHolder.saveItem("color");
ItemsHolder.saveAll();
```

`increase` and `decrease` add and subtract respectively from the item's value.
`toggle` flips the boolean value of the item.

```typescript
ItemsHolder.increase("weight", 14);
ItemsHolder.decrease("weight", 10);
ItemsHolder.toggle("started");
```

`checkExistence` can be used to check if there is an item under the specified key, and if not, add it to the collection.

```typescript
ItemsHolder.checkExistence("color");
```

By default if an item does not exist when `setItem`, `getItem`, `increase`, or `toggle` are called, the item is then added to the collection in the process of the respective function.
Define `allowNewItems` to explicitly enable or disable this feature.

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({ allowNewItems: false });
```

## Updating

When an item's value changes, the module checks its triggers, modularity, and element status.

Triggers are functions that get run when an item's value equals the specified value.

```typescript
ItemsHolder.addItem("color", {
    triggers: {
        red: () => { this.value = "black"; }
    }
});
ItemsHolder.setItem("color", "red");
```

Modularity restricts an item's value to a range from 0 to the specified max and runs the `onModular` function as many times until the value hits the modular range.

```typescript
ItemsHolder.addItem("time", {
    modularity: 12,
    num: 0,
    onModular: () => { this.num += 1; }
});
ItemsHolder.setItem("time", 100);
```

### Auto Save

By default, the values in localStorage are not updated when their values change.
Auto saving which updates localStorage values can be enabled when making the ItemsHolder container.

```typescript
let ItemsHolder = new ItemsHoldr({ autoSave: true });
```

To toggle the autoSave feature, use `toggleAutoSave`.

```typescript
ItemsHolder.toggleAutoSave();
```

## Clearing and Defaults

To clear all items from the collections, use `clear`.

```typescript
ItemsHolder.clear();
```

To clear the collection, but keep some items which will always be in the collection, utilize ItemsHoldr's `valueDefault` property.

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({
    values: {
        color: {
            valueDefault: "red"
        }
    }
});
ItemsHolder.clear(); // "color" is still in the collection with a reset value of "red"
```

## Elements

### *Note:* This part of ItemsHoldr will be placed into another module in the future.

HTML elements stored within ItemsHoldr will have their values on the page updated to their changed value.

```typescript
ItemsHolder.addItem("color", {
    hasElement: true,
    element: {}
});
ItemsHolder.setItem("color", "red");
```

To signal if a container should be made to hold HTML elements, assign a value to `doMakeContainer`.

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({ doMakeContainer: true });
```

With this, any HTML items passed in at the time of construction are held in ItemsHolder's `container` object.
Each of the item's elements are added to the container as its children.
It can be used to display a number of elements grouped together.

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({
    doMakeContainer: true,
    values: {
        color: {
            valueDefault: "red",
            hasElement: true,
            element: {}
        }
    }
});

// color is in the container, but speed is not
ItemsHolder.addItem("speed", {
    valueDefault: 10,
    hasElement: true,
    element: {}
});
```

### Display Changes

If you want certain values to be represented differently on the page, make use of `displayChanges`.
These changes are hardcoded values that replace specified values when element items are updated.

```typescript
let ItemsHolder = new ItemsHoldr({
    displayChanges: {
        Infinity: "INF";
    }
});

ItemsHolder.addItem("limit", {
    value: "4000",
    hasElement: true,
    element: {}
});
ItemsHolder.setItem("limit", "Infinity");
```

`hasDisplayChange` checks to see if a value has a recorded change.

```typescript
ItemsHolder.hasDisplayChange("Infinity");
```

...and `getdisplayChange` returns the entry to replace the value with.

```typescript
let newValue: string = ItemsHolder.getDisplayChange("Infinity");
```

### Container Visibility

`hideContainer` hides the container from view.

```typescript
ItemsHolder.hideContainer();
```

...while `displayContainer` makes the container visible.

```typescript
ItemsHolder.displayContainer();
```

More can be read about ItemsHoldr on its [Readme](https://github.com/FullScreenShenanigans/ItemsHoldr/blob/master/README.md).

# StateHoldr

StateHoldr is a module for tracking changes of items in ItemsHolder in objects called collections.
A collection is an object whose keys are item names in ItemsHolder and whose values are key-value pairs of item attributes and values.
These key-value pairs of attributes and values are referred to as changes.
Collections are like a person and changes are the physical traits describing that person.

## Prefix

Like ItemsHoldr, StateHoldr has its own prefix property.
All collections in StateHoldr are stored in ItemsHoldr prepended with the StateHoldr prefix.
So the collections are stored in `localStorage` prepended with the ItemsHoldr prefix as well.

```typescript
let StateHolder = new StateHoldr({
    ItemsHolder: new ItemsHoldr(),
    prefix: "StateHolder"
});
```

## Collections

Collections allow for various attributes for a single item to be grouped together versus storing individual items.
The collections the module has recorded are stored in a list keyed by `stateCollectionKeys` in ItemsHolder (no StateHoldr prefix prepended).
StateHoldr always has one collection as its current collection.

`setCollection` sets the current collection.

```typescript
// sets the collection name and the collection's value
StateHolder.setCollection("newCollection", {
    car: {
        color: "red"
    }
});

// sets only the name for the collection
StateHolder.setCollection("newCollection");
```

If only a name is given, the collection is set to a blank object.

`saveCollection` saves the current collection to ItemsHolder.

```typescript
StateHolder.saveCollection();
```

## Changes

To add a change to the current collection, use `addChange`.
`addChange` takes in the key of the item, an attribute of the item, and the value being stored.

```typescript
StateHolder.addChange("car", "color", "red");
```

If the name of the collection is known, whether or not it's the current collection, a change can be added with `addCollectionChange`.

```typescript
StateHolder.addCollectionChange("other_collection", "car", "color", "red");
```

To copy changes from an item in the current collection into a target object, use `applyChanges`.

```typescript
let recipient: { [i: string]: any } = {};
StateHolder.applyChanges("car", recipient);
```

`getChanges` returns the changes for a specific item.

```typescript
let changes: { [i: string]: any } = StateHolder.getChanges("car");
```

More can be read about StateHoldr on its [Readme](https://github.com/FullScreenShenanigans/StateHoldr/blob/master/README.md).