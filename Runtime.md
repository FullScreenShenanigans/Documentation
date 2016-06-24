This guide will describe how GamesRunnr runs upkeep functions and how FSPAnalyzer is used to schedule them in GameStartr projects.

### Table of Contents
1. [GamesRunnr](#gamesrunnr)
    1. [Games and Upkeep](#games-and-upkeep)
    2. [Interval and Speed](#interval-and-speed)
    3. [Pause and Play](#pause-and-play)
2. [FPSAnalyzer](#fpsanalyzer)
    1. [Measure](#measure)
    2. [Getters](#getters)

# GamesRunnr

GamesRunnr is a module for running a series of callbacks grouped into one interval.
These callbacks deal with maintaining the state of the game, like having a callback to redraw the canvas.
These callbacks separately are referred to as games.

## Games and Upkeep

`upkeep` is the biggest part of GamesRunnr.
This function runs all the stored games.

`upkeepTimed` is a utility function for `upkeep` that times and returns the amount of time it takes to run all the games.

```typescript
let totalTime: number = GamesRunnr.upkeepTimed();
console.log(`It took `${totalTime}` ms.`);
```

## Interval and Speed

Each call of `upkeep` has a set time delay, in milliseconds, before it can be called again, called an interval.
To set the interval use `setInterval`.

```typescript
GamesRunnr.setInterval(20);
```

`speed` is a variable which represents the playback speed of the intervals, such as a speed of 2 makes the intervals go by twice as fast.
To set the value, use `setSpeed`.

```typescript
GamesRunner.setSpeed(2);
```

## Pause and Play

GamesRunnr can pause and continue `upkeep` execution.
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
    games: [() => console.log("example function")],
    onPause: () => console.log("upkeep is paused"),
    onPlay: () => console.log("upkeep is resumed")
});
GamesRunner.pause();
GamesRunner.play();
```

To toggle between the two, use `togglePause`.

```typescript
GamesRunnr.togglePause();
```

More can be read on GamesRunnr on its [Readme](https://github.com/FullScreenShenanigans/GamesRunnr/blob/master/README.md).

# FPSAnalyzr

## Measure

FSPAnalyzr is a module for recording and analyzing framerate measurements.

The main function for the module is `measure` and is run at the end of every `upkeep` call.

```typescript
FPSAnalyzr.measure();
```

It updates the value for the current timestamp and if there was a previous timestamp recorded, it adds an FPS measurement to the list of recorded measurements with `addFPS`.

```typescript
FPSAnalyzr.addFPS(40);
```

The module records up to `maxKept` number of measurements.

## Getters

FPSAnalyzr has a number of getter functions that return statistics and information on the recorded measurements.

```typescript
// returns the number of allotted measurements
let max: number = FSPAnalyzr.getMaxKept();

// returns the number of measurements currently recorded
let num: number = FSPAnalyzr.getNumRecorded();

// returns the currently recorded timestamp
let current: number = FPSAnalyzr.getTimeCurrent();

// returns the current position in measurements
let position: number = FSPAnalyzr.getTicker();

// returns the recorded measurements
let measurements: number[] = FPSAnalyzr.getMeasurements();

// returns the measurements as time differences
let differences: number[] = FPSAnalyzr.getDifferences();

// returns the average, median, extremes, and range of the recorded FPS measurements
let average: number = FPSAnalyzr.getAverage();
let median: number = FPSAnalyzr.getMedian();
let extremes: number[] = FPSAnalyzr.getExtremes();
let range: number = FPSAnalyzr.getRange();
```

More can be read on FPSAnalyzr on its [Readme](https://github.com/FullScreenShenanigans/FPSAnalyzr/blob/master/README.md).