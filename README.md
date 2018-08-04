# @mightyplow/eleventy-plugin-cache-buster

A plugin for [eleventy](https://github.com/11ty/eleventy/) which adds a unique query parameter to css and js resources. 
This allows to set unlimited caching for those resources and ensure that they get properly reloaded when they change.
See [this article on Cache Busting](https://www.keycdn.com/support/what-is-cache-busting/).

By now the hash is created out of the resource content. See [notes](#notes).

## Installation
````
npm i @mightyplow/eleventy-plugin-cache-buster -D
````

## Usage

In your projects' _.eleventy.js_ file, add the plugin.

````.js
const cacheBuster = require('@mightyplow/eleventy-plugin-cache-buster');

module.exports = function(eleventyConfig) {
    const cacheBusterOptions = {};
    eleventyConfig.addPlugin(cacheBuster(cacheBusterOptions));
};

````

### Options

#### outputDirectory: string

The directory where eleventy stores the built files to. This is required to resolve resources which start with a slash.
If eleventy is called with the --output option from the command line, this option can be omitted.

#### hashParameter: string (default: 'v')

The query parameter which is appended to the resource urls, i.e. _/foo/bar.css?v=[hash]_.

#### sourceAttributes: object (default: { link: 'href', script: 'src' })

An object which maps the relevant html tags to the attributes, in which the url can be found in. By default it is looking
for script tags and link tags.

Since the options get merged, the default attributes don't have to be included when you want to add additional ones.

## Notes

### Valid resource URLs

By now this plugin only works with path-relative URLs.
- what will get processed:
    - `/assets/css/foo.css`
    - `../assets/css/foo.css`

- what will not get processed:
    - `//foo.bar/assets/css/foo.css`
    - `https://foo.bar/assets/css/foo.css`

### Build order

In order to create the resource hashes, the resource files have to be built
before the html. You have to ensure this in your build process.
