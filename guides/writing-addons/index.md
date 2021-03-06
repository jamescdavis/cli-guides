If a developer has worked on an Ember app before, they should feel right at home making an addon! There are many similarities between the two. In this guide, we will cover the step by step instructions.

Writing an addon is a great way to organize code, share it with others, or get the foundational knowledge to contribute to Open Source addons. By separating features into an addon instead of leaving it as an in-app component, more developers can work in parallel and breaking changes can be managed independently. Maintainability goes up, and testing becomes easier.

Since the Ember community has so many addons, one of the best ways to learn more advanced addon development is to study existing addons. If we get stuck or need to see some examples in action, [Ember Observer's code search](https://www.emberobserver.com/code-search) can be very helpful.

Although an addon looks and feels a lot like an Ember app, it is important to work in small steps and validate that each piece is working before writing more code. Developers who are very comfortable with Ember apps might otherwise make a lot of changes and walk into some common pitfalls that can be hard to debug in unison.

### Generating the addon files

Use the Ember CLI to create the file structure for the addon. Run this command in a fresh directory, not inside an existing Ember app:

```shell
ember addon <addon-name> [options]
```

The result is the creation of a directory called `<addon-name>`, which has many files and looks a bit like an Ember app. We won't need to use all the files to make a useful addon. By convention, _most_ Ember addons start with `ember` in the name, like `ember-basic-dropdown`. This will help other developers find our addon.

### Addon file structure

In some ways, an addon is like a mini Ember app. It has a very similar file structure, uses a lot of the same API methods, and can do most things that components are able to do.

Let's take a look a some of the most important files and folders in an addon, and how they are different from what we would find in an app.

#### `addon/`

This directory can hold many of the same subdirectories and files that an Ember app would, like `/components/` and `/templates/`. For developers who are making components, most of the work will happen here.

#### `app/`

The `app` directory plays an important role to help an Ember app automatically discover the components exported by an addon.
The default way to make a component is to put the implementation in `addon/`, which allows developers to import and extend the addon component. However, Ember apps always look for components within the `app` namespace, so we must re-export our components from `app/`.

Fortunately, when we run `ember generate component my-component-name` in an addon project, the CLI takes care of all this re-exporting business. It creates the necessary files and code for us. Addon authors don't usually need to think about the `app` directory or do any work in it.

#### `public/`

You can add static assets in the `public/` directory in your addon, and they will automatically be
merged into the public directory in the host app under a folder with the name of your addon.

For example, if your addon is published to npm as `@foo/my-awesome-addon`, and it contains an
image at `public/icon.png`, the host app can reference it as
`/@foo/my-awesome-addon/icon.png` in CSS or JS.

#### `index.js`

An addon will leverage npm conventions and look for an `index.js` as the entry point, unless another entry point is specified via the "main" property in the `package.json` file. You are encouraged to use `index.js` as the addon entry point for your addon. `index.js` is also where you add hooks if you're doing something intermediate or advanced with your addon.

#### `ember-cli-build.js`

The ember-cli-build.js inside your addon is only used to configure the dummy application found in `tests/dummy/`. It is never referenced by applications which include the addon.

If you need to use `ember-cli-build.js`, you may have to specify paths relative to the addon root directory. For example, to configure `outputPaths` in the dummy app:

```javascript {data-filename=ember-cli-build.js}
const EmberAddon = require('ember-cli/lib/broccoli/ember-addon');

module.exports = function(defaults) {
  let app = new EmberAddon(defaults, {
    outputPaths: {
      app: {
        js: '/assets/main.js'
      }
    }
  });

  return app.toTree();
};
```

#### `tests/dummy/`
This directory contains a full Ember app for addon testing purposes. During tests, we can check to make sure that the addon works or looks as expected when it is used in an app. Many addon developers use the dummy app to hold their documentation site's content as well.

#### `package.json`

If we want other people to be able to use our addon, we need to specify a name, license, version, the repository URL, and description. For an addon to show up on [Ember Observer](https://emberobserver.com), it must have `keywords: ["ember-addon"]` and a repository URL.

#### `config/ember-try.js`

This is a place to configure which versions of Ember that the test suite should check for compatibility. See the [ember-try](https://github.com/ember-cli/ember-try) repository on GitHub for more information.

#### `config/environment.js`

Values here will be defaults for apps that use our addon. Any changes in the apps own `environment.js` will overwrite these defaults.

##### Example of default configuration:
```javascript {data-filename=config/enviroment.js}
let ENV = {
  'your-awesome-addon': {
    awesomeLevel: 11,
    outputDebugInfo: false,
  },
};
```
