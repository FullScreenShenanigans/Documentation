This guide will describe how FullScreenShenanigans projects keep track of game information.

### Table of Contents
1. [ItemsHoldr](#itemsholdr)
    1. [Local Storage](#local-storage)
    2. [Items](#items)
    3. [Updating](#updating)
    4. [Clearing and Defaults](#clearing-and-defaults)
    5. [Elements](#elements)
2. [StateHoldr](#stateholdr)
    1. [Prefix](#prefix)
    2. [Collections](#collections)
    3. [Changes](#changes)

# ItemsHoldr

ItemsHoldr is a versatile container to store and manipulate values in localStorage, and optionally keep an HTML container to show these values.

## Local Storage

ItemsHoldr keeps track of items and their values while the game is running.
It can also save this information to `localStorage`.
Other websites also use `localStorage`, so ItemsHoldr has a prefix property to differentiate ItemsHoldr's information.

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({
    prefix: "FullScreenShenanigans"
});
```

## Items

To add, update, and remove an item in the collection, use `addItem`, `setItem`, and `removeItem` respectively.

```typescript
ItemsHolder.addItem("color", { value: "red" });
ItemsHolder.setItem("color", "purple");
ItemsHolder.removeItem("color");
```

`increase` and `decrease` add and subtract from the item's value.
`toggle` flips the boolean value of the item.

```typescript
ItemsHolder.increase("weight", 14);
ItemsHolder.decrease("weight", 10);
ItemsHolder.toggle("started");
```

`checkExistence` can be used to check if there is an item under the specified key and if not, add it to the collection.

```typescript
ItemsHolder.checkExistence("color");
```

By default if an item does not exist when `setItem`, `getItem`, `increase`, or `toggle` are called, the item is then added to the collection.
Define `allowNewItems` to explicitly enable or disable this feature.

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({
    allowNewItems: false
});
```

To manually update a changed item's value in localStorage, use `saveItem`.
To save all the items' values to localStorage, use `saveAll`.

```typescript
ItemsHolder.saveItem("color");
ItemsHolder.saveAll();
```

### Auto Save

Auto saving, which updates an item's value in localStorage when the value is changed, can be enabled when making the ItemsHolder container.

```typescript
let ItemsHolder = new ItemsHoldr({
    autoSave: true
});
```

By default, this is disabled.
To toggle autoSave, use `toggleAutoSave`.

```typescript
ItemsHolder.toggleAutoSave();
```

## Updating

Triggers are functions that get run when an item's value equals the specified value.

```typescript
ItemsHolder.addItem("color", {
    value: "cyan",
    triggers: {
        red: () => { this.value = "black"; }
    }
});
ItemsHolder.setItem("color", "red");
```

Modularity restricts an item's value to a range from 0 to the specified max.
When the value is changed, the `onModular` function is run `x / modularity` times, where `x` is the number the value was set to.
Each time the function is run, value gets set to `value % modularity`.

```typescript
ItemsHolder.addItem("counter", { value: 0 });
ItemsHolder.addItem("time", {
    value: 0,
    modularity: 12,
    onModular: () => { ItemsHolder.increase("counter", 1); }
});
ItemsHolder.setItem("time", 100);
```

## Clearing and Defaults

To clear all items from the collection, use `clear`.

```typescript
ItemsHolder.clear();
```

To clear the collection, but keep some items which will always be in the collection, use `valueDefault`.

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

*Note:* This part of ItemsHoldr will be placed into another module in the future.

To signal if a container should be made to hold HTML elements, assign a value to `doMakeContainer`.

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({
    doMakeContainer: true
});
```

Changes to element values will show on the page if the container is appended to an element on the page.
To signal if an item is an element, assign `hasElement` to true.

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({
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

```typescript
let ItemsHolder = new ItemsHoldr({
    displayChanges: {
        Infinity: "INF";
    }
});

ItemsHolder.addItem("limit", {
    value: "4000",
    hasElement: true
});
ItemsHolder.setItem("limit", "Infinity");
```

`hasDisplayChange` checks to see if a value has a recorded change and `getDisplayChange` returns the entry to replace the value with.

```typescript
ItemsHolder.hasDisplayChange("Infinity");
let newValue: string = ItemsHolder.getDisplayChange("Infinity");
```

### Container Visibility

`hideContainer` hides the container from view and `displayContainer` makes the container visible.

```typescript
ItemsHolder.hideContainer();
ItemsHolder.displayContainer();
```

More can be read about ItemsHoldr on its [Readme](https://github.com/FullScreenShenanigans/ItemsHoldr/blob/master/README.md).

# StateHoldr

StateHoldr is a module for tracking changes of items in ItemsHolder in objects called collections.
Changes are key-value pairs of attributes for items and values.
A collection describes how named items in a group have been changed. For each item in the group, the collection will store a key-value pair of attributes and new values.
For example, a garage collection that contains a "car" item with "color" and "name" changes could be described as:

```typescript
let garage: { [i:string]: any } = {
    car: {
        color: "red"
    }
};
```

Collections allow for various attributes for a single item to be grouped together versus storing individual items.

## Prefix

Like ItemsHoldr, StateHoldr has its own prefix property.
All collections in StateHoldr are stored in ItemsHoldr prepended with the StateHoldr prefix.

```typescript
let StateHolder: IStateHoldr = new StateHoldr({
    ItemsHolder: new ItemsHoldr(),
    prefix: "StateHolder"
});
```

## Collections

The collections the module has recorded are stored in a list keyed by `stateCollectionKeys` in ItemsHolder (no StateHoldr prefix prepended).
StateHoldr always has one collection as its current collection.

`setCollection` sets the current collection.

```typescript
StateHolder.setCollection("newCollection", {
    car: {
        color: "red"
    }
});
```

If the name passed in is the name of a collection that already exists, that collection's old values will be used.

```typescript
StateHolder.setCollection("newCollection", {
    car: {
        color: "red"
    }
});

StateHolder.setCollection("secondCollection");
StateHolder.setCollection("newCollection"); // car is still in this collection
```

Collections aren't put into ItemsHolder until they are saved.
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

A change to a specific collection can be added with `addCollectionChange`.

```typescript
StateHolder.addCollectionChange("otherCollection", "car", "color", "red");
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