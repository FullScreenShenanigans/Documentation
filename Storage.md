This guide will describe how FullScreenShenanigans projects store and keep track of game information.

### Table of Contents
1. [ItemsHoldr](#itemsholdr)
    1. [Local Storage](#local-storage)
    2. [Items](#items)
    3. [Getters and Setters](#getters-and-setters)
    4. [Clearing and Defaults](#clearing-and-defaults)
    5. [Elements](#elements)
2. [StateHoldr](#stateholdr)

# ItemsHoldr

ItemsHoldr is a versatile container to store and manipulate values in localStorage, and optionally keep an updated HTML container showing these values.

## Local Storage

Local Storage is a browser property that saves information past session activity.
ItemsHoldr keeps track of items and their values while the game is running and periodically saves them to `localStorage`.

Because not just these FSS projects might save to the browser's localStorage, ItemsHoldr has a prefix property to differentiate which information belongs to which source (project or website).

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({
    prefix: "FullScreenShenanigans"
});
```

## Items

Items can be information on something like a player's speed or even on some of the elements of the page, like a button that shows whether the game is muted or not.

### ItemValue

ItemsValue is the custom data type for each item stored.
Items are represented as key value pairs and know the ItemsHoldr container they are stored in.

```typescript
let item: IItemValue = new ItemValue(new ItemsHoldr(), "color", { value: "red" });
```

`setValue` sets the passed in value as the new value for the item.

```typescript
let item: IItemValue = new ItemValue(new ItemsHoldr(), "color", { value: "red" });
item.setValue("blue");
```

`getValue` returns the item's value.

```typescript
let item: IItemValue = new ItemValue(new ItemsHoldr(), "color", { value: "red" });
let value: string = item.getValue()
```

#### Updating

`update` is run whenever the item's value changes.
It checks an item's triggers, modularity, and element status.

Triggers are functions that get run when an item's value equals the specified value.

```typescript
let item: IItemValue = new ItemValue(new ItemsHoldr(), "color", { 
    value: "red",
    triggers: {
        red: function () {
            this.value = "black";
        }
    }
});
item.update(); // the value is now black
```

Modularity limits an item's value to a range from 0 to the specified max.

```typescript
let item: IItemValue = new ItemValue(new ItemsHoldr(), "color", { 
    value: 100,
    modularity: 15,
    num: 0,
    onModular: function () {
        this.num += 1;
    }
});
item.update();
```

If the item is an HTML element, the value is also updated on the page.

```typescript
let item: IItemValue = new ItemValue(new ItemsHoldr(), "color", { 
    valueDefault: "red",
    hasElement: true,
    element: {}
});
item.update();
```

### Auto Save

By default, values are not updated in localStorage when their values change.
Auto saving can be enabled when making the ItemsHolder container.

```typescript
let ItemsHolder = new ItemsHoldr({ autoSave: true });
```

To toggle autoSave use `toggleAutoSave`.

```typescript
ItemsHolder.toggleAutoSave();
```

## Getters and Setters

ItemsHoldr has a number of functions that return information about the colleciton of items or a specific item.

```typescript
// simple gets
ItemsHolder.getKeys();
ItemsHolder.getValues();
ItemsHolder.exportItems();
ItemsHolder.getAutoSave();
ItemsHolder.getLocalStorage();
ItemsHolder.getPrefix();
ItemsHolder.getContainer();
ItemsHolder.getContainersArguments();
ItemsHolder.getDisplayChanges();

// returns the indexed key
ItemsHolder.key(1);
// returns the value under the key
ItemsHolder.getItem("color");
// returns the ItemValue object corresponding with the specified key
ItemsHolder.getObject("color");
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

To update a changed item's value in localStorage, use `saveItem`.

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

Make use of `allowNewItems` to specify whether or not new items can be added from `setItem`, `getItem`, `increase`, and `toggle` which is true by default.

```typescript
let ItemsHolder: ItemsHoldr = new ItemsHoldr({ allowNewItems: false });
```

## Clearing and Defaults

To clear all items from the collections, use `clear`.

```typescript
ItemsHolder.clear();
```

To clear the collection, but keep some items which are always in the collection, utilize ItemHoldr's `valueDefault` property.

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

HTML elements can be stored within ItemsHoldr and their value will be updated on the page when changed.
To signal a container should be made to hold HTML elements, assign a value to `doMakeContainer`.

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

You don't have to make any extra function calls as this is done under the hood.
The constructor calls `makeContainer` which takes in an array of an array of an element type and any number of objects whose properties are copied to the created element.

```typescript
ItemsHolder.makeContainer([
    ["div", {
        className: this.prefix + "_container"
    }]
]);
```

Related functions are `createElement` which is the function that makes the HTML elements.
It takes in a tag for the HTML element and any number of objects to copy properties from.

```typescript
ItemsHolder.createElement("div", {
    className: this.prefix + "_container"
});
```

### Proliferating

Proliferating is the process of deep copying an object's properties to another.
`proliferate` and `proliferateElement` both copy properties from a donor object to a recipient object.
The difference between the two is that `proliferateElement` is tailored specifically to copy properties from an HTML element.

```typescript
ItemsHolder.prolieferate({}, { color: "red" });

let element: HTMLElement = document.create("div");
ItemsHolder.proliferateElement({}, element);
```

### Display Changes

If you want certain values to be represented differently on the page, make use of `displayChanges`.
They are hardcoded values that replace specified values when element items are updated.

```typescript
let ItemsHolder = new ItemsHoldr({
    displayChanges: {
        Infinity: "∞";
    }
});

ItemsHolder.addItem("limit", 4000);
ItemHolder.setItem("limit", "Infinity");
```

`hasDisplayChange` checks to see if a value has a recorded change.

```typescript
ItemsHolder.hasDisplayChange("Infinity");
```

...and `getdisplayChange` returns the entry to replace value with.

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