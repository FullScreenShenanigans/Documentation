Builds are managed by [gulp-shenanigans](https://github.com/FullScreenShenanigans/gulp-shenanigans/).
See the documentation there for how to build, test, and run individual projects.

## Referencing module updates locally

When updating project with changes that should be reflected in other projects, use [`npm link`](https://docs.npmjs.com/cli/link) to create symlinks within `node_modules` directories.

For example, to make `C:/Code/QuadsKeepr`'s `node_modules/ObjectMakr` point to `C:/Code/ObjectMakr`:

```cmd
cd C:/Code/ObjectMakr
npm link

cd C:/Code/QuadsKeepr
npm link ObjectMakr
```

### `shenanigans-manager`

The `shenanigans-manager` npm package may also be useful for larger-scale package management.
For example, to set up a directory with all relevant FullScreenShenanigans repositories linked to each other:

```cmd
npm install -g shenanigans-manager

shenanigans-manager complete-setup --directory C:/Code/Shenanigans
```

*(`complete-setup` will take a while)*
