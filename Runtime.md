This guide will describe how GamesRunnr runs upkeep functions and how FSPAnalyzer is used to schedule them in GameStartr projects.

# GamesRunnr

GamesRunnr is a module for running a series of callbacks grouped into one interval.
These callbacks deal with maintaining the state of the game, like having a callback to redraw the canvas.
These callbacks separately are referred to as games.

## Upkeep

`upkeep` is a function that runs each game once.
`upkeepTimed` is a utility function for `upkeep` that times and returns the amount of time it takes to run all the games.

```javascript
const totalTime: number = gamesRunner.upkeepTimed();
console.log(`It took `${totalTime}` ms.`);
```

## Interval and Speed

Each call of `upkeep` has a set time delay, in milliseconds, before it can be called again, called an interval.
To set the interval use `setInterval`.

```javascript
gamesRunner.setInterval(20);
```

`speed` is a variable which represents the playback speed of the intervals, for example a speed of 2 makes the intervals go by twice as fast.
To set the value, use `setSpeed`.

```javascript
gamesRunner.setSpeed(2);
```

## Pause and Play

GamesRunnr can pause and continue `upkeep` execution.
`pause` stops the execution of `upkeep` and cancels the next call of upkeep.

```javascript
gamesRunner.pause();
```

`play` continues execution of `upkeep` by calling it.

```javascript
gamesRunner.play();
```

Both `pause` and `play` allow for trigger functions to run when they are called by defining `onPause` and `onPlay`.

```javascript
const gamesRunner = new GamesRunnr({
    games: [() => console.log("example function")],
    onPause: () => console.log("upkeep is paused"),
    onPlay: () => console.log("upkeep is resumed")
});
gamesRunner.pause();
gamesRunner.play();
```

To toggle between the two, use `togglePause`.

```javascript
gamesRunner.togglePause();
```

More can be read on GamesRunnr on its [Readme](https://github.com/FullScreenShenanigans/GamesRunnr/blob/master/README.md).


# FPSAnalyzr

## Measure

FSPAnalyzr is a module for recording and analyzing framerate measurements.

`measure` and is run at the end of every `upkeep` call.

```javascript
fpsAnalyzer.measure();
```

It updates the value for the current timestamp and if there was a previous timestamp recorded, it adds an FPS measurement to the list of recorded measurements with `addFPS`.

```javascript
fpsAnalyzer.addFPS(40);
```

## Getters

FPSAnalyzer has a number of getter functions that return statistics and information on the recorded measurements.

```javascript
fpsAnalyzer.getNumRecorded();
fpsAnalyzer.getTicker();
fpsAnalyzer.getMeasurements();
fpsAnalyzer.getDifferences();
fpsAnalyzer.getAverage();
fpsAnalyzer.getMedian();
fpsAnalyzer.getExtremes();
fpsAnalyzer.getRange();
```

More can be read on FPSAnalyzr on its [Readme](https://github.com/FullScreenShenanigans/FPSAnalyzr/blob/master/README.md).
