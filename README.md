# jmespath.js

[![Build Status](https://travis-ci.org/jmespath/jmespath.js.png?branch=master)](https://travis-ci.org/jmespath/jmespath.js)

jmespath.js is a javascript implementation of JMESPath,
which is a query language for JSON.  It will take a JSON
document and transform it into another JSON document
through a JMESPath expression.

Using jmespath.js is really easy.  There's a single function
you use, `jmespath.search`:


```
> var jmespath = require('jmespath');
> jmespath.search({foo: {bar: {baz: [0, 1, 2, 3, 4]}}}, "foo.bar.baz[2]")
2
```

In the example we gave the ``search`` function input data of
`{foo: {bar: {baz: [0, 1, 2, 3, 4]}}}` as well as the JMESPath
expression `foo.bar.baz[2]`, and the `search` function evaluated
the expression against the input data to produce the result ``2``.

The JMESPath language can do a lot more than select an element
from a list.  Here are a few more examples:

```
> jmespath.search({foo: {bar: {baz: [0, 1, 2, 3, 4]}}}, "foo.bar")
{ baz: [ 0, 1, 2, 3, 4 ] }

> jmespath.search({"foo": [{"first": "a", "last": "b"},
                           {"first": "c", "last": "d"}]},
                  "foo[*].first")
[ 'a', 'c' ]

> jmespath.search({"foo": [{"age": 20}, {"age": 25},
                           {"age": 30}, {"age": 35},
                           {"age": 40}]},
                  "foo[?age > `30`]")
[ { age: 35 }, { age: 40 } ]
```

## Compiled Expressions
```
var fooBar = jmespath.compile("foo.bar");
fooBar.search({foo: {bar: {baz: [0, 1, 2, 3, 4]}}})

//Evaluates to { baz: [ 0, 1, 2, 3, 4 ] }
```

## Custom Functions
```
function keyValue(resolvedArguments) {
    var data = resolvedArguments[0];
    return Object.keys(data).map(key => (
        { key: key, value: data[key]}
    ));
}

var customFunctions = { 
    "key_values": { 
        _func: keyValue,
        _signature: [{types: [TYPE_OBJECT]}]
    }
};
jmespath.search({foo: {bar: "a", baz: [0, 1, 2, 3, 4]}}, "key_values(foo)", { customFunctions })

//Evaluates to [ {key: "foo", value: "a"}, {key: "baz", value: [0, 1, 2, 3, 4]} ]
//Alternatively, defaultOptions is exported and customFunctions can be set there which will apply to all jmespath.search
```

## More Resources

The example above only show a small amount of what
a JMESPath expression can do.  If you want to take a
tour of the language, the *best* place to go is the
[JMESPath Tutorial](http://jmespath.org/tutorial.html).

One of the best things about JMESPath is that it is
implemented in many different programming languages including
python, ruby, php, lua, etc.  To see a complete list of libraries,
check out the [JMESPath libraries page](http://jmespath.org/libraries.html).

And finally, the full JMESPath specification can be found
on the [JMESPath site](http://jmespath.org/specification.html).
