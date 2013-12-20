# Node Style Guide

- Style
- Example
- Testing
- Version Switching

## Style

- Use four spaces for indenting your code (do not use tabs)
- Use `\n` for a newline character (yes this is unix specifc, do not use the Window format `\r\n`)
- No trailing white space (you should be able to configure your editor to do this automatically)
- Use semi-colons
- Use braces (with the exception of short/concise one line `if` statements - see below example)
- Conditionals/Loops should have spacing around parenthesis
- Functions should have single space between right side parenthesis and corresponding bracket
- Opening braces should go on the same line (see below example)
- Use single quotes for Strings `'my string'` (NOT double quotes `"my string"`)
- Use strict equality `===`
- Declare one variable per `var` statement (NOT single `var` statement for multiple variables `var a = 1, b = 2, c = 3;` as this makes re-ordering variables more prone to error because of stray commas)
- Don't nest callbacks, specify an identifier instead and extract the callback into its own function declaration
- Try to keep lines of code to eighty characters per line. This makes it easier to read and understand the code.
- Use lowerCamelCase for variables, properties and function names
- Use UpperCamelCase for class names
- Use UPPERCASE for Constants
- Only quote Object keys if your interpreter complains (see below example)
- Use descriptive conditions/queries (e.g. a complex regex no one can understand should be placed inside a variable which describes the outcome)
- Write small functions
- Return early from functions (i.e. fail fast)
- If you do use a closure then don't nest them, and make sure to name them (this provides better stack trace information)
- Use slashes for comments and only for side effects and edge case information (otherwise, refactor your code so it doesn't need comments to explain itself)

## Example

```js
var x = require('x');
var y = require('y');
var z = require('z');

function doTask(callback) {
    asyncFunction1('foo', {
        bar: 'baz',
        'needs quoting': 123
    }, onCompletion);

    function onCompletion(err, res) {
        if (err) return callback(err)

        asyncFunction2('baz', onCompletionAgain);
    }

    function onCompletionAgain(err, res) {
        if (err) return callback(err);

        asyncFunction3('bar', onCompletionFinal);

        function onCompletionFinal(err, res) {
            if (err) return callback(err);
            
            if (res === 'someThing') {
                callback(null, 'data to passback to the original callback function')
            }
        }
    }
}

req.on('end', function onEnd() {
    console.log('see we named our closure `onEnd` which will help us when debugging');
});
```

## Testing

For unit testing your Node.js code we recommend a light wrapper around the built-in `Assert` module called [NodeUnit](https://github.com/caolan/nodeunit)

## Version Switching

There are two popular Node version managers...

- [NVM](https://github.com/creationix/nvm) - Node Version Manager - Simple bash script to manage multiple active node.js versions
- [Nave](https://github.com/isaacs/nave) - Virtual Environments for Node
