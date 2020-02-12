### In-Repo-Addons

Addons specific to your project can be created inside your repo and are
generated in the projects `lib` directory in a folder with the name of
the in-repo addon, e.g. `/lib/in-repo-addon-name` and follow the same
file structure as a normal *addon*.

Some advantages of using an in-repo addon are:

- Sandbox for developing a feature that you may want to share as an
  addon in the future
- Having all the benefits of addon isolation but without the need to
  publish or `npm link`

Use `in-repo-addon` argument with the `ember generate` command:

```bash
ember generate in-repo-addon in-repo-addon-name
```

(Replace `in-repo-addon-name` with the name of your addon.)

### Using a stylesheet with an in-repo-addon

For your in-repo addon stylesheet, name the file `addon.css` and place
it in the styles directory, e.g `/lib/in-repo-addon-name/addon/styles/addon.css`
This avoids any conflict with the parent application's `app.css` file

Likewise if your Ember CLI application uses `.less` or `.scss`, use the
appropriate file extension for your addon stylesheet file.

### Using templates with an in-repo-addon
In order to compile HTMLBars templates that are part of your in-repo-addon,
your app's `package.json` file will need to include following dependencies:

- `babel-plugin-htmlbars-inline-precompile`
- `ember-cli-babel`
- `ember-cli-htmlbars`
- `ember-cli-htmlbars-inline-precompile`

(Use the same versions found in your Ember CLI Application's `package.json`)

### Dependencies

Generally speaking, an in-repo addon should avoid dependencies in its package.json.
If you find that your in-repo addon has many dependencies, consider making a full-fledged ember
addon instead. Additionally, if a dependency exists in the project's `package.json`, they are
available in the in-repo addon (regardless of the in-repo addon's package.json).

However, in situations where in-repo addons add dependencies, take care to note whether the
dependency is in the `dependencies` or `devDependencies` section. The former will be added
to the final `vendor.js`, and the latter will be available at build time. Note that development
dependencies that are themselves addons, *can* also show up in the final production Javascript build.

### In Addons

In-repo addons defined in addons generally work the same way as in-repo addons in apps.

### Broccoli build options for in-repo-addons

To ensure that you can use babel.js and related polyfills with your in-repo-addon,
add babel options to the `included` hook of the in-repo-addon `index.js`:

```javascript
module.exports = {
  name: 'in-repo-addon-name',

  isDevelopingAddon() {
    return true;
  },

  included(app, parentAddon) {
    let target = (parentAddon || app);
    target.options = target.options || {};
    target.options.babel = target.options.babel || { includePolyfill: true };
    return this._super.included.apply(this, arguments);
  }
};
```

### Generating an in-repo-addon blueprint

To generate a blueprint for your in-repo-addon:

```bash
ember generate blueprint <blueprint-name> --in-repo-addon <in-repo-addon-name>
```

When generating a blueprint, a shorthand argument `-ir` or `-in-repo` can be
used in place of `--in-repo-addon`.
