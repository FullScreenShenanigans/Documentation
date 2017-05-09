This is the general getting started guide for all FullScreenShenanigans projects. The guide will show you how to download and run any FullScreenShenanigans game as well as teach you about how to use the console to interact with the game.


## Development

First set up your system using the steps in [Development](https://github.com/FullScreenShenanigans/Documentation/blob/master/Things.md).


## Using the Console

You can use your browser's developer tools to interact with the game.
FullScreenShenanigans projects store their game instances globally with an acronym for the project title.

```javascript
FSP.gamesRunner.pause();
setTimeout(() => FSP.gamesRunner.play(), 1000);
```

In this case, `FSP` is the acronym for FullScreenPokemon.


## Tutorials

There's no definitive guide through the documentation yet.
For now, read through guides in this order:

1. [Components and Modules](https://github.com/FullScreenShenanigans/Documentation/blob/master/Components%20and%20Modules.md)
2. [Things](https://github.com/FullScreenShenanigans/Documentation/blob/master/Things.md)
3. [Inputs](https://github.com/FullScreenShenanigans/Documentation/blob/master/Inputs.md)
4. [Runtime](https://github.com/FullScreenShenanigans/Documentation/blob/master/Runtime.md)
5. [Events](https://github.com/FullScreenShenanigans/Documentation/blob/master/Events.md)
7. Any remaining guides in any order.
