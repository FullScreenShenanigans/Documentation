This is the build description guide that explains the build process for all FullScreenShenanigans projects. The guide will explain to you how projects are built and why they are built that way. Please note that this guide only fully applies to the GameStartr games, as some of these tasks are not run when building the modules.

### Table of Contents

1. [How to Build](#how-to-build)
2. [Grunt Tasks](#grunt-tasks)

## How to Build
In order to build a FullScreenShenanigans project you must use Grunt. To do so, open the terminal and navigate to the project directory. You can build the project all at once or step-by-step.

If you wish to build it in one go, enter `grunt`. This command will run all of the tasks specified in the [Grunt Tasks](#grunt-tasks) section of this document.

Alternatively, you can run each grunt task manually. This allows you to run part of the build at a time. In order to run an individual task, type `grunt task_name` and press enter. For example, if you want to run the clean task, enter `grunt clean`. The different tasks are outlined in the [Grunt Tasks](#grunt-tasks) section of this document.


## Grunt Tasks
The following ten tasks are run, in order, every time you build the project with grunt. The end result of the build is a new directory named Distribution. This directory will contain all of the project files in minified form.

### clean
The clean task deletes all files and folders in the project's Distribution directory. This occurs because the Distribution folder must be recreated to reflect any changes made to the files.

See the [grunt-contrib-clean page](https://github.com/gruntjs/grunt-contrib-clean) for more information.

### tslint
In order to make sure that the .ts files in the project are neat and readable, grunt runs tslint. This task checks each file for whitespaces, alignment issues, unused variables, missing semicolons, and other stylistic problems, throwing errors if it finds any issues.

See the [grunt-tslint page](https://github.com/palantir/grunt-tslint) for more information.

### typescript
#### typescript:base
This task compiles the .ts files in Source into new .js files and adds the new files to Source.
#### typescript:distribution
The typescript:distribution task translates the project's main .ts file into a .js file that is placed in the Distribution directory. The new file's name is concatenated with the version number of the project.

See the [grunt-typescript page](https://github.com/k-maru/grunt-typescript) for more information.

### copy
This task copies the following files and folders from the Source directory into the Distribution directory. 
* The project's main .ts file. The project version number is concatenated to the file name when added to Distribution.
* The main .d.ts file, also concatenated with the version number. 
* The .js file created by typescript:distribution. This is copied to Distribution/{Project Name}-version. 
* The References directory.
* The Fonts, Sounds, and Theme directories.

The README.md and LICENSE.txt files are copied to Source, as well as Distribution.

See the [grunt-contrib-copy page](https://github.com/gruntjs/grunt-contrib-copy) for more information.

### uglify
The uglify:dist task minifies the .js files. The files are compressed into two files: One of them contains the file created when typescript:distribution was run along with all .js file in the settings and maps directories while the other file minifies index.js. A sourcemap is created for each of these new files.

See the [grunt-contrib-uglify page](https://github.com/gruntjs/grunt-contrib-uglify) for more information.

### cssmin
cssmin:dist minifies .css files. It compresses index.css and creates a sourcemap for the newly compressed file. Both of these files can be found in the Distribution directory.

See the [grunt-contrib-cssmin page](https://github.com/gruntjs/grunt-contrib-cssmin) for more information.

### preprocess
In order to preprocess the file created in the typescript:distribution task, preprocess:dist is run. This task will carry out the directives at the top of the file. It will print out the paths for several modules and include these modules, the .d.ts file, and the Cutscenes.d.ts files in the build of the file.

See the [grunt-preprocess page](https://github.com/jsoverson/grunt-preprocess) for more information.

### processhtml
The processhtml:dist task processes the build commands given in Source/index.html. The task builds index.css, index.js, and the project's main .min.js file. It also removes the block of code that imports all of the .js files, making the minified index.html file much smaller.

See the [grunt-processhtml page](https://github.com/dciccale/grunt-processhtml) for more information.

### htmlmin
htmlmin:dist minifies the index.html file in the Distribution directory.

See the [grunt-contrib-htmlmin page](https://github.com/gruntjs/grunt-contrib-htmlmin) for more information.

### mocha_phantomjs
mocha_phantomjs:all runs tests using the Mocha testing framework in the PhantomJS command-line-accessible browser.

See the [grunt-mocha-phantomjs page](https://github.com/jdcataldo/grunt-mocha-phantomjs) for more information.