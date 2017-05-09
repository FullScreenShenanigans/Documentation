# Development

Development on projects under the FullScreenShenanigans organization is generally managed by two packages:
* **[`shenanigans-manager`](https://github.com/FullScreenShenanigans/shenanigans-manager)** sets up your computer with cloned packages pointing to each other.
* **[`gulp-shenanigans`](https://github.com/FullScreenShenanigans/gulp-shenanigans)** provides [Gulp](http://gulpjs.com/) build tasks for each package.

## Initial Setup

First, install the prerequisite software:
* [Node.js](http://node.js.org)
* [Git](https://git-scm.com/downloads)

It's recommended to use [Visual Studio Code](https://code.visualstudio.com/) as your editor with the [TSLint extension](https://marketplace.visualstudio.com/items?itemName=eg2.tslint) installed.


### `shenanigans-manager`

Use `shenanigans-manager`'s `complete-setup` command to set up a directory with all the relevant FullScreenShenanigans projects linked to each other.

Using a shell / command prompt with administrative privileges:

```shell
npm install -g shenanigans-manager gulp
shenanigans-manager complete-setup --directory C:/Code/Shenanigans
```

After this, all the repositories will be fully built.
Their `node_modules` dependencies will by symbolically linked to each other.

You can launch Code in a project by starting it on the command-line and passing it a directory.

```shell
code C:/Code/Shenanigans/AreaSpawnr
```

See the [`shenanigans-manager` documentation](https://github.com/FullScreenShenanigans/shenanigans-manager) for details on the available commands.

## Building

Full builds can be launched with `gulp`, though those are too slow for general development.
The recommended way to build is to start Gulp watching your source files:

```shell
gulp watch
```

This will trigger incremental compilation whenever a source file changes.

### `src` and `test`

The `tsconfig.json` for files under `/src` is located in the root of the project, but a different one exists at `/test/tsconfig.json` for test files.
The two directories have separate build steps and will both output compiled files next to their source equivalents.

You can run tests using `gulp test` or by opening `/test/index.html` in a browser.

See the [`gulp-shenanigans` documentation](https://github.com/FullScreenShenanigans/gulp-shenanigans/blob/master/README.md#builds) for details on how to build, test, and run individual projects.
