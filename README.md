# The Problem

You've got some thing where you need to push a bunch of stuff into a
queue and then shift it out.  Or, maybe it's a stack, and you're just
pushing and popping it.

Arrays work for this, but are a bit costly performance-wise.

# The Solution

A linked-list implementation that takes advantage of what v8 is good at:
creating objects with a known shape.

This is faster for this use case.  How much faster?  About 50%.

    $ node bench.js
    benchmarking /Users/isaacs/dev-src/js/fast-list/bench.js
    Please be patient.
    { node: '0.6.2-pre',
      v8: '3.6.6.8',
      ares: '1.7.5-DEV',
      uv: '0.1',
      openssl: '0.9.8l' }
    Scores: (bigger is better)

    new FastList()
    Raw:
     > 24390.243902439026
     > 25072.324011571844
     > 24505.183788878418
     > 24551.463644948064
     > 24738.344433872502
    Average (mean) 24651.511956341972

    []
    Raw:
     > 12248.468941382327
     > 12227.07423580786
     > 12237.762237762237
     > 12227.07423580786
     > 12195.121951219513
    Average (mean) 12227.100320395959

    new Array()
    Raw:
     > 12142.237640936686
     > 12216.404886561955
     > 12248.468941382327
     > 12205.754141238012
     > 12079.378774805868
    Average (mean) 12178.44887698497

    Winner: new FastList()
    Compared with next highest ([]), it's:
    50.4% faster
    2.02 times as fast
    0.3 order(s) of magnitude faster

    Compared with the slowest (new Array()), it's:
    50.6% faster
    2.02 times as fast
    0.31 order(s) of magnitude faster

This lacks a lot of features that arrays have:

1. You can't specify the size at the outset.
2. It's not indexable.
3. There's no join, concat, etc.

If any of this matters for your use case, you're probably better off
using an Array object.

## Installing

```
npm install fast-list
```

## API

```javascript
var FastList = require("fast-list")
var list = new FastList()
list.push("foo")
list.unshift("bar")
list.push("baz")
console.log(list.length) // 2
console.log(list.pop()) // baz
console.log(list.shift()) // bar
console.log(list.shift()) // foo
```

### Methods

* push: Just like Array.push, but only can take a single entry
* pop: Just like Array.pop
* shift: Just like Array.shift
* unshift: Just like Array.unshift, but only can take a single entry
* drop: Drop all entries
