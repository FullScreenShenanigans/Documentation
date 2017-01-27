# Things

This is a general guide about in-game Things for all FullScreenShenanigans projects.

## Things

Every in-game object in GameStartr games is a Thing.
Things and their classes are generated using [ObjectMakr](https://github.com/FullScreenShenanigans/ObjectMakr), which generates classes based on the inheritance structure given to it by its module settings.

```javascript
const thing = FSP.objectMaker.make("MyThing");
```

Most of the basic properties, like `height`, `width`, and `opacity` are defined in the parent Thing class.
ObjectMakr's `make` is a shortcut to initializing a new instance of a Thing class and assigning properties to it in one step.

More can be read on ObjectMakr on its [Readme](https://github.com/FullScreenShenanigans/ObjectMakr/blob/master/README.md).

## Groups

Each Thing has a `.groupType` string defining which "group" of Things it belongs to.
These groups are stored within the GroupHoldr module, which contains an array or object for each group.
Things are added to their respective group when added to the game and removed when they die.

### Getting groups

The groups are accessible both by static name and via passing in a String.

```javascript
// Returns the Solid group
FSP.groupHolder.getGroup("Solid");

// Returns the first Solid
FSP.groupHolder.getGroup("Solid")[0];
```

More can be read on GroupHoldr on its [Readme](https://github.com/FullScreenShenanigans/GroupHoldr/blob/master/README.md).


## Physics

A Thing's placement on the screen is based on its `top`, `right`, `bottom`, and `left` properties.
These define the bounding box for a Thing.
A Thing is essentially a rectangle, with its height, width, and location defined by these properties.

The coordinate system in which these Things are placed has an origin at the top left corner (0,0).

There are a number of ways to move Things, all of which are done by manipulating these 4 properties.

```javascript
// Instantiates Things used in the following code samples.
const thing = FSP.GroupHolder.getGroup("Solid")[0];
const otherThing = FSP.GroupHolder.getGroup("Solid")[1];
```

Physics methods are stored under the `physics` member of a GameStartr instance.

### Shift

You can use the shift functions `shiftVert` and `shiftHoriz`.
These move the Thing up and down or left and right.

```javascript
// Shifts a Thing vertically by 1.
FSP.physics.shiftVert(thing, 1);

// Shifts a Thing horizontally by 1.
FSP.physics.shiftHoriz(thing, 1);
```

### Set

The `set` functions (`setTop`, `setRight`, `setBottom`, or `setLeft`) moves the Thing to line up with the bounding property.

```javascript
// Sets the top bound of a Thing to 10.
FSP.physics.setTop(thing, 10);

// Sets the left bound of a Thing to 10.
FSP.physics.setLeft(thing, 10);

// Sets the bottom bound of a Thing to 10.
FSP.physics.setBottom(thing, 10);

// Sets the right bound of a Thing to 10.
FSP.physics.setRight(thing, 10);
```  

### Midpoints

It's possible to set the Thing's midpoint using `setMidX`, `setMidY`, and `setMid` (the latter of which sets both the `x` and `y` midpoints).
These functions make it so the Thing is centered on the given `x` and `y`.

```javascript
// Sets both x and y midpoints to 1 and 2, respectively.
FSP.physics.setMid(thing, 1, 2);

// The above is equivalent to the following 2 function calls.
FSP.physics.setMidX(thing, 1);
FSP.physics.setMidY(thing, 2);
```

Going along with setting midpoints, you can do that based on the midpoint of another Thing.
Using `setMidXObj`, `setMidYObj`, and `setMidObj` you can shift a Thing so that its horizontal and/or vertical midpoint(s) are centered on that of another Thing.

```javascript
// Aligns a Thing's horizontal and vertical midpoints with those of anotherThing.
// This is equivalent to the following 2 function calls combined.
FSP.physics.setMidObj(thing, otherThing);

// Aligns just the horizontal midpoint.
FSP.physics.setMidXObj(thing, otherThing);

// Aligns just the vertical midpoint.
FSP.physics.setMidYObj(thing, otherThing);
```

### Slide

`slideToX` or `slideToY` will slide a Thing toward a target `x` or `y`, while limiting the total distance allowed (distance computed from the Thing's original midpoint).

```javascript
// Slides a Thing to x position 10, moving a max distance of 5.
FSP.physics.slideToX(thing, 10, 5);

// Slides a Thing to y position 10, moving a max distance of 5.
FSP.physics.slideToY(thing, 10, 5);
```


## Opacity

Opacity defines how transparent a Thing is.
It's defined in the `opacity` property and ranges from 0 to 1 (0 = fully transparent, 1 = fully visible).

You can set a Thing's opacity using `setOpacity`.

```javascript
// Sets a Thing's opacity to 1.
FSP.physics.setOpacity(thing, 1);

// Sets a Thing's opacity to 0 (transparent).
FSP.physics.setOpacity(thing, 0);
```


## Adding Things

You're able to add Things to the game via `addThing`, specifying the `x` and `y` positions (again this is relative to the top left corner of the screen).
The `x` is where the Thing's `left` bound will be and the `y` is where the Thing's `top` bound will be.
You can add it using a GameStartr instance's `things` component and the class title for the Thing:

```javascript
// Adds a Thing to the game at position (1,2) using the class name.
const first = FSP.things.add("FenceWide", 1, 2);

// Adds a Thing to the right of another Thing.
const second = FSP.things.add("FenceWide", first.right, first.top);

// Adds a Thing below another Thing.
FSP.things.add("FenceWide", second.left, second.bottom);
```
