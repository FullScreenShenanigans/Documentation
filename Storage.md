This guide will describe how FullScreenShenanigans projects store and keep track of game information.

### Table of Contents
1. [ItemsHoldr](#itemsholdr)
    1. [Local Storage](#local-storage)
    2. [Getters and Setters](#getters-and-setters)
    3. [Updating](#updating)
    4. [Clearing and Defaults](#clearing-and-defaults)
    5. [Elements](#elements)
2. [StateHoldr](#stateholdr)

# ItemsHoldr

ItemsHoldr is a versatile container to store and manipulate values in localStorage, and optionally keep an updated HTML container showing these values.

## Local Storage

Local Storage is a browser property where information is saved past session activity.
ItemsHoldr keeps track of items and their values while the game is running and periodically saves them to `localStorage`.

Because not just these FSS projects might be using the browser's localStorage, ItemsHoldr has a prefix property to differentiate which information belongs to which source.

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({
    prefix: "FullScreenShenanigans"
});
```

## Getters and Setters

ItemsHoldr has a number of functions that return information about the collection of items or a specific item.

```typescript
// simple gets
ItemsHolder.getKeys();
ItemsHolder.getValues();
ItemsHolder.exportItems();
ItemsHolder.getAutoSave();
ItemsHolder.getLocalStorage();
ItemsHolder.getPrefix();
ItemsHolder.getContainer();
ItemsHolder.getDisplayChanges();

// returns the indexed key
ItemsHolder.key(1);
// returns the value under the key
ItemsHolder.getItem("color");
// returns whether the key is in the collection
ItemsHolder.hasKey("color");
```

To add an item to the collection, use `addItem`.

```typescript
ItemsHolder.addItem("color", { value: "red" });
```

To remove an item, use `removeItem`.

```typescript
ItemsHolder.removeItem("color");
```

`setItem` is used to set the value of an item.

```typescript
ItemsHolder.setItem("color", "purple");
```

To manually update a changed item's value in localStorage, use `saveItem`.

```typescript
ItemsHolder.saveItem("color");
```

To save all the items' values in localStorage, use `saveAll`

```typescript
ItemsHolder.saveAll();
```

`increase` and `decrease` add and subtract respectively from the item's value.

```typescript
ItemsHolder.increase("weight", 14);
ItemsHolder.decrease("weight", 10);
```

`toggle` flips the boolean value of the item.

```typescript
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
ItemsHolder.setItem("color", red);
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

If the item is an HTML element, the value is also updated on the page.

```typescript
ItemsHolder.addItem("color", {
    hasElement: true,
    element: {}
});
ItemsHolder.setItem("color", "red");
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

To clear the collection, but keep some items which will always be in the collection, utilize ItemHoldr's `valueDefault` property.

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

HTML elements stored within ItemsHoldr will have their values updated on the page when changed.
To signal if a container should be made to hold HTML elements, assign a value to `doMakeContainer`.

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({ doMakeContainer: true });
```

With this, any HTML items in the collection at the time of construction are held in ItemHolder's `container` object.

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
    haselement: true,
    element: {}
});
```

### Display Changes

If you want certain values to be represented differently on the page, make use of `displayChanges`.
These changes are hardcoded values that replace specified values when element items are updated.

```typescript
let ItemsHolder = new ItemsHoldr({
    displayChanges: {
        Infinity: "∞";
    }
});

ItemsHolder.addItem("limit", {
    value: "4000",
    haselement: true,
    element: {}
});
ItemHolder.setItem("limit", "Infinity");
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

More can be read about StateHoldr on its [Readme](https://github.com/FullScreenShenanigans/StateHoldr/blob/master/README.md).