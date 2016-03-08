This is a general guide about Things for all FullScreenShenanigans projects. This guide will show you how in-game Things are stored and manipulated. Everything you see in the game (trees, blocks, the player, etc.) is a Thing.  
The Thing class is subclassed by a new class for everything (Tree class, Block class, Player class, etc.). These Things can be manipulated to change their location and properties.  
All the following code samples are written specific to `FSM` (FullScreenMario) but work for all FullScreenShenanigans projects when adjusted accordingly (switching FSM out for FSP, etc.).  
  
### Table of Contents  
1. [GroupHoldr](#groupholdr)
    1. [Getting Groups](#getting-groups)
2. [Manipulating Things](#manipulating-things)
    1. [Shifting Things](#shifting-things)
    2. [Opacity](#opacity)

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