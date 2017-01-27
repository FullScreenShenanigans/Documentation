This is the general getting started guide for all FullScreenShenanigans projects. The guide will show you how to download and run any FullScreenShenanigans game as well as teach you about how to use the console to interact with the game.


## Obtaining the Source Code

All FullScreenShenanigans projects are within the FullScreenShenanigans organization on GitHub.
For example, FullScreenPokemon is available at https://github.com/FullScreenShenanigans/FullScreenPokemon.

Use Git to clone the repository from GitHub:

1. If you don't already have Git installed on your computer, go to [this page](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and follow the instructions to download and install Git.
2. Follow [these directions](https://help.github.com/articles/cloning-a-repository/) to clone the repository.
3. See [Build Process](#build-process) to see how to build and play the game.


## Build Process

FullScreenShenanigans projects use [Gulp](http://gulpjs.com/) to build, which requires you to [download and install Node.js](http://nodejs.org) first.

Go to the root repository folder and run the following commands:

```cmd
npm install -g gulp
npm install
gulp
```

You can now play the game by opening src/index.html in a browser.

See [gulp-shenanigans](https://github.com/FullScreenShenanigans/gulp-shenanigans) for details on build processes.

### Using the Console

You can use your browser's developer tools to interact with the game.
FullScreenShenanigans projects store their game instances globally with an acronym for the project title.

```javascript
FSP.gamesRunner.pause();
setTimeout(() => FSP.gamesRunner.play(), 1000);
```

In this case, `FSP` is the acronym for FullScreenPokemon.


## Development

There's no definitive guide through the documentation yet.
For now, read through guides in this order:

1. [Components and Modules](https://github.com/FullScreenShenanigans/Documentation/blob/master/Components%20and%20Modules.md)
2. [Things](https://github.com/FullScreenShenanigans/Documentation/blob/master/Things.md)
3. [Inputs](https://github.com/FullScreenShenanigans/Documentation/blob/master/Inputs.md)
4. [Runtime](https://github.com/FullScreenShenanigans/Documentation/blob/master/Runtime.md)
5. [Events](https://github.com/FullScreenShenanigans/Documentation/blob/master/Events.md)
7. Any remaining guides in any order.
