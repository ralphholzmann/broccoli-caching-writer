
## 0.5.0

* Allow filtering on files to include/exclude when determining when to invalidate the cache. This allows
  you to use simple regular expressions to prevent invalidating the cache when files that do not affect the
  tree in question are changed.

```javascript
var outputTree = compileCompass(inputTree, {
  filterFromCache: {
    include: [
      /.(scss|sass)$/   // only base the input tree’s hash on *.scss and *.sass files
    ]
  }
});
```

  This does _not_ affect what files make it to the output tree at all, rather it only makes it easier
  for subclasses to only rebuild when file types they care about change.

* Symlink from cache instead of manually hard-linking. This should be a speed improvement
  for posix platforms, and will soon be able to take advantage of improvements for Windows
  (for those curious stay tuned on Windows support [here](https://github.com/broccolijs/node-symlink-or-copy/pull/1)).

* Allow multiple input trees. If an array of trees is passed to the constructor, all trees will be read and their collective
  output will be used to calculate the cache (any trees invalidating causes `updateCache` to be called). 

  The default now is to assume that an array of trees is allowed, if you want to opt-out of this behavior set `enforceSingleInputTree`
  to `true` on your classes prototype.

  By default an array of paths will now be the first argument to `updateCache` (instead of a single path in prior versions). The
  `enforceSingleInputTree` property also controls this.

* Due to the changes above (much more being done in our constructor), inheritors are now required to call the `broccoli-caching-writer`
  constructor from their own.
