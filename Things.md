This is a general guide about Things for all FullScreenShenanigans projects. This guide will show you how in-game Things are stored and manipulated. Everything you see in the game (trees, blocks, the player, etc.) is a Thing.  
The Thing class is subclassed by a new class for everything (Tree class, Block class, Player class, etc.). These Things can be manipulated to change their location and properties.  
All the following code samples are written specific to `FSM` (FullScreenMario) but work for all FullScreenShenanigans projects when adjusted accordingly (switching FSM out for FSP, etc.).

### Table of Contents  
1. [GroupHoldr](#groupholdr)
    1. [Getting Groups](#getting-groups)
2. [Manipulating Things](#manipulating-things)
    1. [Shifting Things](#shifting-things)
    2. [Opacity](#opacity)
3. [Adding Things](#adding-things)
4. [TimeHandlr](#timehandlr)
5. [Thing Classes](#thing-classes)
6. [Sprites](#sprites)
    1. [Sprite Cycles and Movement](#sprite-cycles-and-movement)

# GroupHoldr  

GroupHoldr is a module that's used to store similarly typed information, such as Things. A module is a member of the game's instance that controls certain aspects of that instance. Each Thing has a `groupType` string property that determines what group it's considered to be in. For example, the groups in `FSM` are:

* Text
* Character
* Solid
* Scenery 

GroupHoldr contains an Array for each of the groups; each Array contains all the Things of that type currently in the game. Things are added to their respective group when added to the game and removed when they die. For example, in `FSM`, solid objects like floors and blocks would be stored in the Solid group. 

### Getting Groups  
The groups are accessible both by static name and via passing in a String.  

```javascript
// Returns the Solid group
FSM.GroupHolder.getGroup("Solid");

// Returns the first Solid
FSM.GroupHolder.getGroup("Solid")[0];
FSM.GroupHolder.getSolid(0);
```

More can be read on GroupHoldr on its [Readme](https://github.com/FullScreenShenanigans/GroupHoldr/blob/master/README.md).

# Manipulating Things

## Shifting Things
A Thing's placement on the screen is based on its `top`, `right`, `bottom`, and `left` properties. These define the bounding box for a Thing. A Thing is basically just a rectangle, with its height, width, and location defined by these properties.

The coordinate system in which these Things are placed has an origin at the top left corner (0,0). Things are also scaled from these raw measurements to actual in-game measurements using a predefined `unitsize` (i.e. a `unitsize` of 1 means no scaling; a `unitsize` of 2 means in-game measurements are doubled).

There are a number of ways to move Things, all of which are done by manipulating these 4 properties.

```javascript
// Instantiates Things used in the following code samples.
var thing = FSM.GroupHolder.getSolid(0);
var otherThing = FSM.GroupHolder.getSolid(1);
```

#### Shift

You can use the shift functions `shiftVert` and `shiftHoriz`. These move the Thing up and down or left and right.

```javascript
// Shifts a Thing vertically by 1.
FSM.shiftVert(thing, 1);

// Shifts a Thing horizontally by 1. 
FSM.shiftHoriz(thing, 1);
```

#### Set

The `set` functions (`setTop`, `setRight`, `setBottom`, or `setLeft`) moves the Thing to line up with the bounding property.

```javascript
// Sets the top bound of a Thing to 10. 
FSM.setTop(thing, 10);

// Sets the left bound of a Thing to 10. 
FSM.setLeft(thing, 10);

// Sets the bottom bound of a Thing to 10. 
FSM.setBottom(thing, 10);

// Sets the right bound of a Thing to 10. 
FSM.setRight(thing, 10);
```  

#### Midpoints

It's possible to set the Thing's midpoint using `setMidX`, `setMidY`, and `setMid` (the latter of which sets both the `x` and `y` midpoints). These functions make it so the Thing is centered on the given `x` and `y`.

```javascript
// Sets both x and y midpoints to 1 and 2, respectively. 
FSM.setMid(thing, 1, 2);

// The above is equivalent to the following 2 function calls. 
FSM.setMidX(thing, 1);
FSM.setMidY(thing, 2);
```

Going along with setting midpoints, you can do that based on the midpoint of another Thing. Using `setMidXObj`, `setMidYObj`, and `setMidObj` you can shift a Thing so that its horizontal and/or vertical midpoint(s) are centered on that of another Thing.

```javascript
// Aligns a Thing's horizontal and vertical midpoints with that of anotherThing.
// This is equivalent to the following 2 function calls combined.
FSM.setMidObj(thing, otherThing);

// Aligns just the horizontal midpoint. 
FSM.setMidXObj(thing, otherThing);

// Aligns just the vertical midpoint. 
FSM.setMidYObj(thing, otherThing);
```

#### Slide

`slideToX` or `slideToY` will slide a Thing toward a target `x` or `y`, while limiting the total distance allowed (distance computed from the Thing's original midpoint).

```javascript
// Slides a Thing to x position 10, moving a max distance of 5. 
FSM.slideToX(thing, 10, 5);

// Slides a Thing to y position 10, moving a max distance of 5. 
FSM.slideToY(thing, 10, 5);
```

## Opacity

Opacity defines how transparent a Thing is. It's defined in the `opacity` property and ranges from 0 to 1 (0 = fully transparent, 1 = fully visible).

You can set a Thing's opacity using `setOpacity`.

```javascript
// Sets a Thing's opacity to 1. 
FSM.setOpacity(thing, 1);

// Sets a Thing's opacity to 0 (transparent). 
FSM.setOpacity(thing, 0);
```

# Adding Things

You're able to add Things to the game via `addThing`, specifying the `x` and `y` positions (again this is relative to the top left corner of the screen). The `x` is where the Thing's `left` bound will be and the `y` is where the Thing's `top` bound will be. You can add it using the class title for the Thing:

```javascript
// Adds a Thing to the game at position (1,2) using the class name.
FSM.addThing("CastleFireball", 1, 2);

// Adds a Thing to the right of another Thing.
FSM.addThing("CastleFireball", otherThing.right, otherThing.top);

// Adds a Thing below another Thing.
FSM.addThing("CastleFireball", otherThing.left, otherThing.bottom);
```

# TimeHandlr

TimeHandlr is a module used for setting timed events. It's a more flexible alternative to JavaScript's `setTimeout` and `setInterval`. It schedules events in time slots that are handled at each game upkeep, which is fired every 16 ms.

You can use it to add your own events using `addEvent`. The first two arguments to `addEvent` specify what function to call and after how many upkeeps, respectively. Any arguments passed to addEvent after these two are passed to the callback function (the first argument).

```javascript
// Sets a Thing to shift right by 2 after 100 game upkeeps.
FSM.addEvent(FSM.shiftHoriz, 100, thing, 2);
```

In terms of the above example, `thing` and `2` are passed as the arguments to `shiftHoriz` when it's called (after 100 upkeeps).

Events can also be added to repeat, similar to using `setInterval`.  A new third argument, `numRepeats`, is what specifies how many times to repeat, and anything after that are the arguments to the callback function. The event will repeat every however many upkeeps, that many times, until it's repeated the specified number of times or the callback function returns `true`. `Infinity` is allowed to be passed for `numRepeats`, which will cause the event to continuously repeat until it returns `true`.

```javascript
// Sets a Things to shift right by 2 after 100 game upkeeps, repeating 5 times.
FSM.addEventInterval(FSM.shiftHoriz, 100, 5, thing, 2);
```

More can be read about TimeHandlr on its [Readme](https://github.com/FullScreenShenanigans/TimeHandlr/blob/master/README.md).

# Thing classes

Classes are generated using [ObjectMakr](https://github.com/FullScreenShenanigans/ObjectMakr), a module that dynamically generates the inheritance structure of classes. It takes in the structure and a list of properties and the class objects are then constructed using this information. 

When a FullScreenShenanigans game starts up, ObjectMakr receives the inheritance structure and properties list from an `objects.js` file. It then generates prototypes for all of the internal classes (Box, Tree, etc.). Most of the basic properties, like `height`, `width`, and `opacity` are defined in the parent Thing class. ObjectMakr's `addThing` is a shortcut to initializing a new instance of a Thing class and assigning properties to it in one step. 

More can be read about ObjectMakr on its [ReadMe](https://github.com/FullScreenShenanigans/ObjectMakr/blob/master/README.md).

# Sprites

Sprites comprise any visual you see in a FullScreenShenanigans game. They are stored as string representations of an image, containing information about every pixel. The game generates the image from this string in real-time.

More can be read about sprites on [PixelDrawer's](https://github.com/FullScreenShenanigans/PixelDrawr/blob/master/README.md) and [PixelRendr's](https://github.com/FullScreenShenanigans/PixelRendr/blob/master/README.md) ReadMe's.

Each Thing class has a set of sprites relevant to it, usually different views (facing forward, to the left, etc.). The game selects which sprite to use based on the Thing's full class nomenclature (i.e. if the Player is facing left, its class could be "Player left").

## Sprite Cycles and Movement

A sprite cycle is a timed change in a Thing's class handled by TimeHandlr. This is represented by a cycling through of selected sprites, where each phase lasts a specified time interval. 

Sprite cycles are often used to simulate movement. For example, you can show a character walking by adding a cycle between the walking and standing sprites.

```javascript
// Adds a walking/standing cycle to a Thing, cycling every 100 game ticks.
// "Moving" is the name of the cycle, to be referenced in the thing's .cycles property.
FSM.TimeHandler.addClassCycle(
    thing,
    ["walking", "standing"],
    "moving",
    100);
```