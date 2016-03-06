This is the general building guide for all FullScreenShenanigans projects. The guide will show you how to download and run any FullScreenShenanigans game as well as teach you about how to use the console to interact with the game.

### Table of Contents

1. [Obtaining the Source Code](#obtaining-the-source-code)
2. [Build Process](#build-process)
3. [Using the Console](#using-the-console)

## Obtaining the Source Code

All FullScreenShenanigans projects follow the following URL format: https://github.com/FullScreenShenanigans/*project-name*/. For example, if you want to download your own copy of FullScreenMario, you can get it at https://github.com/FullScreenShenanigans/FullScreenMario/. 

In order to play the game, you need to use one of the two following methods to obtain the source code:

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

FullScreenShenanigans projects use [Grunt](http://gruntjs.com/) to automate building, which requires you to [download and install Node.js](http://nodejs.org) first.

Go to the root repository folder and run the following commands:
    
    npm install -g grunt-cli
    npm install
    grunt

You can now play the game by opening Source/index.html in a browser (preferably Google Chrome).

Every time grunt is run, it carries out a set of tasks requested in the repository. In the case of the FullScreenShenanigan projects, grunt is used to:

1. Create a new Distribution folder in the root.
2. Convert .ts files into .js files.
3. Compress the project's main .js file.

It's also zipped for your convenience.


## Using the Console
You can use the console of the F12 Developer Tools to interact with the game. Open the console by pressing `Ctrl+Shift+J` on Windows, or `Cmd+Opt+J` on Mac, at the same time. In order to use the console, enter a command and press `Enter`. A few examples of console use when playing FullScreenMario are pausing the game

```javascript
FSM.GamesRunner.pause();
```
and unpausing the game
```javascript
FSM.GamesRunner.play();
```

In this case, `FSM` is the acronym for FullScreenMario. If you are playing a different FullScreenShenanigans project, replace `FSM` with the acronym for that game. You have access to all functions found in the FullScreenShenanigans project's .ts file and can therefore add and remove Things, change around the maps and more.