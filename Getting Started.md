This is the general getting started guide for all FullScreenShenanigans projects. The guide will show you how to download and run any FullScreenShenanigans game as well as teach you about how to use the console to interact with the game.


## Obtaining the Source Code

All FullScreenShenanigans projects are within the FullScreenShenanigans organization on GitHub. For example, FullScreenPokemon is available at https://github.com/FullScreenShenanigans/FullScreenPokemon 

In order to play a game, you need to use one of the two following methods to obtain the source code:

### Clone the project repository using GitHub:
1. If you don't already have Git installed on your computer, go to [this page](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and follow the instructions to download and install Git.
2. Follow [these directions](https://help.github.com/articles/cloning-a-repository/) to clone the repository.
3. See [Build Process](#build-process) to see how to build and play the game.

### Download the latest release .zip of the project:
1. Go to the GitHub page of your desired game.
2. Click the `Download ZIP` button found on the right side of the page.
3. Extract the .zip into a directory of your choice.
4. Open Source/index.html in a browser (preferably Google Chrome).


## Build Process

FullScreenShenanigans projects use [Gulp](http://gulpjs.com/) to build, which requires you to [download and install Node.js](http://nodejs.org) first.

Go to the root repository folder and run the following commands:
    
    npm install -g gulp
    npm install
    gulp

You can now play the game by opening lib/index.html in a browser.

See [gulp-shenanigans](https://github.com/FullScreenShenanigans/gulp-shenanigans) for details on build processes.


## Using the Console
You can use the console of the F12 Developer Tools to interact with the game. Open the console by using the `Ctrl+Shift+J` short on Windows or `Cmd+Opt+J` on Mac. In order to use the console, enter a command and press `Enter`. A few examples of console use when playing FullScreenPokemon are pausing the game

```javascript
FSP.GamesRunner.pause();
```
and unpausing the game
```javascript
FSP.GamesRunner.play();
```

In this case, `FSP` is the acronym for FullScreenPokemon. If you are playing a different FullScreenShenanigans project, replace `FSP` with the acronym for that game. 
