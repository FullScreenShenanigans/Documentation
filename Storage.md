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
const itemsHolder: IItemsHoldr = new ItemsHoldr({
    prefix: "FullScreenShenanigans"
});
```

## Items

To add, update, and remove an item in the collection, use `addItem`, `setItem`, and `removeItem` respectively.

```typescript
itemsHolder.addItem("color", { value: "red" });
itemsHolder.setItem("color", "purple");
itemsHolder.removeItem("color");
```

`increase` and `decrease` add and subtract from the item's value.
`toggle` flips the boolean value of the item.

```typescript
itemsHolder.increase("weight", 14);
itemsHolder.decrease("weight", 10);
itemsHolder.toggle("started");
```

`checkExistence` can be used to check if there is an item under the specified key and if not, add it to the collection.

```typescript
itemsHolder.checkExistence("color");
```

By default if an item does not exist when `setItem`, `getItem`, `increase`, or `toggle` are called, the item is then added to the collection.
Define `allowNewItems` to explicitly enable or disable this feature.

```typescript
const itemsHolder: IItemsHoldr = new ItemsHoldr({
    allowNewItems: false
});
```

To manually update a changed item's value in localStorage, use `saveItem`.
To save all the items' values to localStorage, use `saveAll`.

```typescript
itemsHolder.saveItem("color");
itemsHolder.saveAll();
```

### Auto Save

Auto saving, which updates an item's value in localStorage when the value is changed, can be enabled when making the ItemsHolder container.

```typescript
const itemsHolder = new ItemsHoldr({
    autoSave: true
});
```

By default, this is disabled.
To toggle autoSave, use `toggleAutoSave`.

```typescript
itemsHolder.toggleAutoSave();
```

## Updating

Triggers are functions that get run when an item's value equals the specified value.

```typescript
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

```typescript
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

```typescript
itemsHolder.clear();
```

To clear the collection, but keep some items which will always be in the collection, use `valueDefault`.

```typescript
const itemsHolder: IItemsHoldr = new ItemsHoldr({
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
const itemsHolder: IItemsHoldr = new ItemsHoldr({
    doMakeContainer: true
});
```

Changes to element values will show on the page if the container is appended to an element on the page.
To signal if an item is an element, assign `hasElement` to true.

```typescript
const itemsHolder: IItemsHoldr = new ItemsHoldr({
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
const itemsHolder: IItemsHoldr = new ItemsHoldr({
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

```typescript
itemsHolder.hasDisplayChange("Infinity");
const newValue: string = ItemsHolder.getDisplayChange("Infinity");
```

### Container Visibility

`hideContainer` hides the container from view and `displayContainer` makes the container visible.

```typescript
itemsHolder.hideContainer();
itemsHolder.displayContainer();
```

More can be read about ItemsHoldr on its [Readme](https://github.com/FullScreenShenanigans/ItemsHoldr/blob/master/README.md).

# StateHoldr

StateHoldr is a module for tracking changes of items in ItemsHolder in objects called collections.
Changes are key-value pairs of attributes for items and values.
A collection describes how named items in a group have been changed. For each item in the group, the collection will store a key-value pair of attributes and new values.
For example, a garage collection that contains a "car" item with "color" and "name" changes could be described as:

```typescript
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

```typescript
const stateHolder: IStateHoldr = new StateHoldr({
    ItemsHolder: new ItemsHoldr(),
    prefix: "StateHolder"
});
```

## Collections

The collections the module has recorded are stored in a list keyed by `stateCollectionKeys` in ItemsHolder (no StateHoldr prefix prepended).
StateHoldr always has one collection as its current collection.

`setCollection` sets the current collection.

```typescript
stateHolder.setCollection("newCollection", {
    car: {
        color: "red"
    }
});
```

If the name passed in is the name of a collection that already exists, that collection's old values will be used.

```typescript
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

```typescript
stateHolder.saveCollection();
```

## Changes

To add a change to the current collection, use `addChange`.
`addChange` takes in the key of the item, an attribute of the item, and the value being stored.

```typescript
stateHolder.addChange("car", "color", "red");
```

A change to a specific collection can be added with `addCollectionChange`.

```typescript
stateHolder.addCollectionChange("otherCollection", "car", "color", "red");
```

To copy changes from an item in the current collection into a target object, use `applyChanges`.

```typescript
const recipient: { [i: string]: string } = {};
stateHolder.applyChanges("car", recipient);
```

`getChanges` returns the changes for a specific item.

```typescript
const changes: { [i: string]: string } = StateHolder.getChanges("car");
```

More can be read about StateHoldr on its [Readme](https://github.com/FullScreenShenanigans/StateHoldr/blob/master/README.md).
