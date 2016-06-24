This guide will describe how GamesRunnr runs upkeep functions FSPAnalyzer is used to schedule them in GameStartr projects.

### Table of Contents
1. [GamesRunnr](#gamesrunnr)
    1. [Games](#games)
    2. [Interval and Speed](#interval-and-speed)
    3. [Pause and Play](#pause-and-play)
2. [FPSAnalyzer](#fpsanalyzer)

# GamesRunnr

GamesRunnr is a module for running a series of callbacks on timed intervals.
These callbacks deal with maintaining the state of the game.
An example being a callback to redraw the canvas.
These callbacks separately are referred to as games.

## Games

`upkeep` is the biggest part of GamesRunnr.
This function deals with running all the stored games.

`upkeepTimed` is a utility function for `upkeep` that times and returns the amount of time it takes to run all the games.

```typescript
let totalTime: number = GamesRunnr.upkeepTimed();
console.log(`It took `${totalTime}` ms.`);
```

This is used to help calculate `upkeepNext` which is a number reference to the next upkeep which is scheduled by `upkeepScheduler`.
`upkeepScheduler` is a custom function that takes in a handler for another function and sets it to run after a delay.

```typescript
GamesRunner.upkeepScheduler(() => console.log("This ran after 200 ms."), 200);
```

`upkeepNext` is like `setTimeout`'s return value where it is possible to cancel the upkeep if passed to `upkeepCanceller` which behaves similarly to `clearTimeout`.

```typescript
let reference: number = GamesRunner.upkeepScheduler(() => console.log("This won't appear in the console."), 200);
GamesRunner.upkeepCanceller(reference);
```

## Interval and Speed

Each call of `upkeep` has an interval between them.
To set the interval use `setInterval`.

```typescript
GamesRunnr.setInterval(20);
```

`speed` is a variable which represents the playback speed of the intervals (e.g. a speed of 2 makes the intervals go by twice as fast).
Having this allows the game to run at 2x, .5x, etc. speed.
To set the value, use `setSpeed`.

```typescript
GamesRunner.setSpeed(2);
```

## Pause and Play

GamesRunnr has the power to pause and continue `upkeep` execution.
`pause` stops the execution of `upkeep` and cancels the next call with `upkeepCanceller`.

```typescript
GamesRunnr.pause();
```

`play` continues execution of `upkeep` by calling it.

```typescript
GamesRunnr.play();
```

Both `pause` and `play` allow for trigger functions to run when they are called by defining `onPause` and `onPlay`.

```typescript
let GamesRunner: IGamesRunnr = new GamesRunnr({
    games: [example: () => console.log("example function")],
    onPause: (runner: IGamesRunnr, callbackArguments: any[]) => console.log("upkeep is paused");,
    onPlay: (runner: IGamesRunnr, callbackArguments: any[]) => console.log("upkeep is resumed");
});
```

To toggle between the two, use `togglePause`.

```typescript
GamesRunnr.togglePause();
```

More can be read on GamesRunnr on its [Readme](https://github.com/FullScreenShenanigans/GamesRunnr/blob/master/README.md).