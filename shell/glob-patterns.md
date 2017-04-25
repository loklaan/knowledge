# glob patterns

Unix-like environments can match filenames with glob patterns.

## common patterns

* `*.js`
* `foo/bar.*`
* `foo/bar.{js,jsx}` or `foo/bar.jsx?`
* `foo/**/*.js`
* `foo-[1-9]/bar.js`
* `foo-[1-9]/bar.js`
* `!foo-[1-9]/bar.js`

## extends patterns

* `?(a|b)` zero or one
* `*(a|b)` zero or more
* `@(a|b)` at least one
* `+(a|b)` one or more

## see also

[https://github.com/isaacs/minimatch](https://github.com/isaacs/minimatch)
[http://www.globtester.com](http://www.globtester.com)
[https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html](https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html)
[https://realguess.net/tags/minimatch/](https://realguess.net/tags/minimatch/)
