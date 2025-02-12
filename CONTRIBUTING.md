# Contributing

**Note:** This guide is a work in progress. Feel free to [submit an issue](https://github.com/Hypercontext/linkifyjs/issues/new) if anything is confusing or unclear.

## How linkify works

<!-- TODO: Insert diagram here -->

Linkify uses a two stage lexicographical analysis to detect patterns in plain text.

The first stage, called the scanner, takes the input string and generates encompassing tokens that aggregate different types of components like domain names and TLDs. For example, substings like `http:` or `com` are converted into tokens named `PROTOCOL` and `TLD`, respectively.

The second stage, the parser, takes this array of tokens and aggregates them into complete entities that are either become links or do not. For example, the tokens `PROTOCOL`, `SLASH`, `SLASH`, `DOMAIN`, `TLD` (appearing in that order) are grouped into a `URL` entity. These groups are called multitokens.

A multitoken is either a link or not a link. Core linkify comes with these multitokens

* **`TEXT`** is plain text (that contains no linkable entities)
* **`NL`** represents a single newline character
* **`EMAIL`** email address
* **`URL`**

The latter two are converted to links. `NL` is in some cases converted to a `<br>` HTML tag.

You can use the Token class system to [create plugins](#building-plugins).

## Style

* ES6 Syntax (except tests for now)
* Hard tabs with a width of 4 characters

As usualy, try to keep it consistent with what's already there.

## Development

### Setup

1. Install the latest LTS version of [Node.js](https://nodejs.org/) (v15 or newer)
2. Fork and clone this repository to your machine
3. Navigate to the clone folder via command-line
4. Install dependencies with NPM:
   ```sh
   npm install
   ```

### Building

Linkify is built and tested in the command line. Build tasks have the following format.

```
npm run build
```

Here are the primary build tasks used for development. See [gulpfile.js](https://github.com/Hypercontext/linkifyjs/blob/master/gulpfile.js) for the complete list.

* **`build`** transpiles ES6 to ES5 (via [Babel](http://babeljs.io/)) from `src/` into `lib/`. The lib folder will be published to [NPM](https://www.npmjs.com/). Also generates browser-ready scripts (globals and [AMD](http://requirejs.org/docs/whyamd.html)) into the `build/` folder.
* **`dist`** copies and compresses the contents of `build/` into `dist/`. The contents of the dist folder will be published to [Bower](http://bower.io/).
* **default** (run `gulp` without arguments) runs `build` and begins watching files for changes (rebuilding when they do)

### Running tests

These tools are used for testing linkify:

* [Mocha](https://mochajs.org/) is our primary test case framework
* [ESLint](https://eslint.org) for code linting
* [Istanbul](https://istanbul.js.org/) for code coverage analysis
* [Karma](http://karma-runner.github.io/) is our browser test runner
* [BrowserStack](https://www.browserstack.com) for cross-browser testing

These are all configured to run in npm scripts. Tasks `npm test` and `npm run lint` are the most basic you can run. Other tasks include:

* `npm run build` converts the `src` ES source code into browser- and Node.js- compatible JavaScript. It also outputs TypeScript definitions.
* `npm run clean` removes all generated files
* `npm run dist` cleans, builds and copies the final browser distribution bundle into the root `dist` directory

### Building plugins

**Caution:** The plugin development API is in its very early stages and only supports very basic plugins. Updated features, APIs, and docs are in the works.

Check out the sample [Hashtag plugin](https://github.com/Hypercontext/linkifyjs/blob/main/packages/linkifyjs/src/plugins/hashtag.js) for an example.
