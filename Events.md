# Events

This is a general guide about in-game events for all FullScreenShenanigans projects.

## TimeHandlr

TimeHandlr is a module used for setting timed events.
It's a more flexible alternative to JavaScript's `setTimeout` and `setInterval`.
It schedules events in time slots that are handled at each game upkeep, which is fired every 16 ms.

You can use it to add your own events using `addEvent`.
The first two arguments to `addEvent` specify what function to call and after how many upkeeps, respectively.

```javascript
// Sets a Thing to shift right by 2 after 100 game upkeeps.
FSP.timeHandler.addEvent(
    (): void => FSP.physics.shiftHoriz(thing, 2),
    100);
```

Events can also be added to repeat, similar to using `setInterval`.
A new third argument, `numRepeats`, is what specifies how many times to repeat, and anything after that are the arguments to the callback function.
The event will repeat every however many upkeeps, that many times, until it's repeated the specified number of times or the callback function returns `true`.
`Infinity` is allowed to be passed for `numRepeats`, which will cause the event to continuously repeat until it returns `true`.

```javascript
// Sets a Things to shift right by 2 after 100 game upkeeps, repeating 5 times.
FSP.timeHandler.addEventInterval(
    (): void => FSP.physics.shiftHoriz(thing, 2),
    100,
    5);
```

More can be read about TimeHandlr on its [Readme](https://github.com/FullScreenShenanigans/TimeHandlr/blob/master/README.md).

## Sprite Cycles

A sprite cycle is a timed change in a Thing's visual class handled by TimeHandlr.
This is represented by a cycling through of selected sprites, where each phase lasts a specified time interval.

Sprite cycles are often used to simulate movement.
For example, you can show a character walking by adding a cycle between the walking and standing sprites.

```javascript
// Adds a walking/standing cycle to a Thing, cycling every 100 game ticks.
// "Moving" is the name of the cycle, to be referenced in the thing's .cycles property.
FSP.timeHandler.addClassCycle(
    thing,
    ["walking", "standing"],
    "moving",
    100);
```
