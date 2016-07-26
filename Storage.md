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

ItemsHoldr is a versatile container to store and manipulate values in localStorage, and optionally keep an HTML container to show these values.

## Local Storage

ItemsHoldr keeps track of items and their values while the game is running.
It can also save this information to `localStorage`.
Other websites also use `localStorage`, so ItemsHoldr has a prefix property to differentiate which information belongs to which source.

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

To manually update a changed item's value in localStorage, use `saveItem`.
To save all the items' values to localStorage, use `saveAll`.

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

`checkExistence` can be used to check if there is an item under the specified key and if not, add it to the collection.

```typescript
ItemsHolder.checkExistence("color");
```

By default if an item does not exist when `setItem`, `getItem`, `increase`, or `toggle` are called, the item is then added to the collection.
Define `allowNewItems` to explicitly enable or disable this feature.

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({ allowNewItems: false });
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

### Auto Save

Auto saving, which updates an item's value in localStorage when the value is changed, can be enabled when making the ItemsHolder container.

```typescript
let ItemsHolder = new ItemsHoldr({ autoSave: true });
```

By default, this is disabled.
To toggle autoSave, use `toggleAutoSave`.

```typescript
ItemsHolder.toggleAutoSave();
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
let ItemsHolder: ItemsHoldr = new ItemsHoldr({ doMakeContainer: true });
```

Changes to element values will show on the page if the container is appended to an element on the page.
To signal if an item is an element, assign `hasElement` to true.

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({
    doMakeContainer: true,
    values: {
        color: {
            valueDefault: "black",
            hasElement: true
        }
    }
});
ItemsHolder.setItem("color", "red");
```

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

More can be read about StateHoldr on its [Readme](https://github.com/FullScreenShenanigans/StateHoldr/blob/master/README.md).