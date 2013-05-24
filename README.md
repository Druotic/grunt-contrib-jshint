# grunt-contrib-jshint [![Build Status](https://travis-ci.org/gruntjs/grunt-contrib-jshint.png?branch=master)](https://travis-ci.org/gruntjs/grunt-contrib-jshint)

> Validate files with JSHint.



## Getting Started
This plugin requires Grunt `~0.4.0`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-contrib-jshint --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-contrib-jshint');
```




## Jshint task
_Run this task with the `grunt jshint` command._

Task targets, files and options may be specified according to the grunt [Configuring tasks](http://gruntjs.com/configuring-tasks) guide.

For more explanations of the lint errors JSHint will throw at you please visit [jslinterrors.com](http://jslinterrors.com/).

### Options

Any specified option will be passed through directly to [JSHint][], thus you can specify any option that JSHint supports. See the [JSHint documentation][] for a list of supported options.

[JSHint]: http://www.jshint.com/
[JSHint documentation]: http://www.jshint.com/docs/

A few additional options are supported:

#### globals
Type: `Object`
Default value: `null`

A map of global variables, with keys as names and a boolean value to determine if they are assignable. This is not a standard JSHint option, but is passed into the `JSHINT` function as its third argument. See the [JSHint documentation][] for more information.

#### jshintrc
Type: `String`
Default value: `null`

If this filename is specified, options and globals defined therein will be used. The `jshintrc` file must be valid JSON and looks something like this:

```json
{
  "curly": true,
  "eqnull": true,
  "eqeqeq": true,
  "undef": true,
  "globals": {
    "jQuery": true
  }
}
```

*Be aware that `jshintrc` settings are not merged with your Grunt options.*

#### extensions
Type: `String`
Default value: `''`

A list of non-dot-js extensions to check.

#### ignores
Type: `Array`
Default value: `null`

A list of files and dirs to ignore. This will override your `.jshintignore` file if set and does not merge.

#### force
Type: `Boolean`
Default value: `false`

Set `force` to `true` to report JSHint errors but not fail the task.

#### reporter
Type: `String`
Default value: `null`

Allows you to modify this plugins output. By default it will use a built-in Grunt reporter. Set the path to your own custom reporter or to one of the built-in JSHint reporters: `jslint` or `checkstyle`.

See also: [Writing your own JSHint reporter.](http://jshint.com/docs/reporter/)

#### reporterOutput
Type: `String`
Default value: `null`

Specify a filepath to output the results of a reporter. If `reporterOutput` is specified then all output will be written to the given filepath instead of printed to stdout.

### Usage examples

#### Wildcards
In this example, running `grunt jshint:all` (or `grunt jshint` because `jshint` is a [multi task](http://gruntjs.com/configuring-tasks#task-configuration-and-targets)) will lint the project's Gruntfile as well as all JavaScript files in the `lib` and `test` directories and their subdirectores, using the default JSHint options.

```js
// Project configuration.
grunt.initConfig({
  jshint: {
    all: ['Gruntfile.js', 'lib/**/*.js', 'test/**/*.js']
  }
});
```

#### Linting before and after concatenating
In this example, running `grunt jshint` will lint both the "beforeconcat" set and "afterconcat" sets of files. This is not ideal, because `dist/output.js` may get linted before it gets created via the [grunt-contrib-concat plugin](https://github.com/gruntjs/grunt-contrib-concat) `concat` task.

In this case, you should lint the "beforeconcat" files first, then concat, then lint the "afterconcat" files, by running `grunt jshint:beforeconcat concat jshint:afterconcat`.

```js
// Project configuration.
grunt.initConfig({
  concat: {
    dist: {
      src: ['src/foo.js', 'src/bar.js'],
      dest: 'dist/output.js'
    }
  },
  jshint: {
    beforeconcat: ['src/foo.js', 'src/bar.js'],
    afterconcat: ['dist/output.js']
  }
});
```

#### Specifying JSHint options and globals

In this example, custom JSHint options are specified. Note that when `grunt jshint:uses_defaults` is run, those files are linted using the default options, but when `grunt jshint:with_overrides` is run, those files are linted using _merged_ task/target options.

```js
// Project configuration.
grunt.initConfig({
  jshint: {
    options: {
      curly: true,
      eqeqeq: true,
      eqnull: true,
      browser: true,
      globals: {
        jQuery: true
      },
    },
    uses_defaults: ['dir1/**/*.js', 'dir2/**/*.js'],
    with_overrides: {
      options: {
        curly: false,
        undef: true,
      },
      files: {
        src: ['dir3/**/*.js', 'dir4/**/*.js']
      },
    }
  },
});
```

#### Ignoring specific warnings

If you would like to ignore a specific warning:

```shell
[L24:C9] W015: Expected '}' to have an indentation at 11 instead at 9.
```

You can toggle it by prepending `-` to the warning id as an option:

```js
grunt.initConfig({
  jshint: {
    ignore_warning: {
      options: {
        '-W015': true,
      },
      src: ['**/*.js'],
    },
  },
});
```


## Release History

 * 2013-05-22   v0.5.4   Fix default reporter to show offending file.
 * 2013-05-19   v0.5.3   [object Object]
 * 2013-05-18   v0.5.2   Fix printing too many erroneous ignored file errors.
 * 2013-05-17   v0.5.1   Fix for when only 1 file is lint free.
 * 2013-05-17   v0.5.0   Bump to jshint 2.0. Add support for .jshintignore files and ignores option Add support for extensions option. Add support for custom reporters and output report to a file.
 * 2013-04-08   v0.4.3   Fix evaluation of predef option when it's an object.
 * 2013-04-08   v0.4.2   Avoid wiping force option when jshintrc is used.
 * 2013-04-06   v0.4.1   Fix to allow object type for deprecated predef.
 * 2013-04-04   v0.4.0   Revert task level options to override jshintrc files.
 * 2013-03-13   v0.3.0   Bump to JSHint 1.1.0. Add force option to report JSHint errors but not fail the task. Add error/warning code to message. Allow task level options to override jshintrc file.
 * 2013-02-26   v0.2.0   Bump to JSHint 1.0
 * 2013-02-15   v0.1.1   First official release for Grunt 0.4.0.
 * 2013-01-18   v0.1.1rc6   Updating grunt/gruntplugin dependencies to rc6. Changing in-development grunt/gruntplugin dependency versions from tilde version ranges to specific versions.
 * 2013-01-09   v0.1.1rc5   Updating to work with grunt v0.4.0rc5. Switching to this.filesSrc api.
 * 2012-10-18   v0.1.0   Work in progress, not yet officially released.

---

Task submitted by ["Cowboy" Ben Alman](http://benalman.com/)

*This file was generated on Thu May 23 2013 21:54:28.*
