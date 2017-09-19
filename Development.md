# Development

First install the prerequisite software:
* [Node.js](http://node.js.org)
* [Git](https://git-scm.com/downloads)

It's recommended to use [Visual Studio Code](https://code.visualstudio.com/) as your editor with the [TSLint extension](https://marketplace.visualstudio.com/items?itemName=eg2.tslint) installed.


## Initial Setup

To work on an individual repository, [fork that repository](https://help.github.com/articles/fork-a-repo/) and clone it to your computer.

```cmd
git clone https://github.com/<your-github-username-here>/<project-you-forked>
```

Navigate to that directory in a terminal/shell and run the following commands to initialize the repository for development.

```cmd
npm install
gulp setup
gulp
```


## Building

Full builds can be launched with `gulp`, though those are too slow for general development.
The recommended way to build is to start Gulp watching your source files:

```shell
gulp watch
```

This will trigger incremental compilation whenever a source file changes.

### `src` and `test`

The root `/tsconfig.json` is used for files under `/src`.
A different one exists at `/test/tsconfig.json` for test files.
The two directories will both output compiled files next to their source equivalents and have separate build steps.

### Testing

You can run tests using `gulp test` or by opening `/test/index.html` in a browser.
If you add, remove, or rename a test file, you'll need to run `gulp setup` to re-initialize the test harness.

See the [`gulp-shenanigans` documentation](https://github.com/FullScreenShenanigans/gulp-shenanigans/blob/master/README.md#builds) for details on how to build, test, and run individual projects.


## `shenanigans-manager`

If you're working across multiple repositories under the FullScreenShenanigans organization, use the `shenanigans-manager` utility.
Passing `complete-setup` command to it can set up all repositories locally with node modules symlinked to each other.

For each forked project, pass `--fork repository=organization`.

Using a shell / command prompt with administrative privileges:

```shell
npm install -g shenanigans-manager gulp
shenanigans-manager complete-setup --directory C:/Code/Shenanigans --fork fullscreenpokemon=username
```

After this, all the repositories will be cloned and fully built.
Their `node_modules` dependencies will be symlinked to each other.

See the [`shenanigans-manager` documentation](https://github.com/FullScreenShenanigans/shenanigans-manager) for details on the available commands.
