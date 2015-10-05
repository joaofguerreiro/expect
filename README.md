# expect

[![build status](https://img.shields.io/travis/mjackson/expect.svg?style=flat-square)](https://travis-ci.org/mjackson/expect)
[![npm package](https://img.shields.io/npm/v/expect.svg?style=flat-square)](https://www.npmjs.org/package/expect)

[expect](https://github.com/mjackson/expect) lets you write better assertions.

When you use expect, you write assertions similarly to how you would say them, e.g. "I expect this value to be equal to 3" or "I expect this array to contain 3". When you write assertions in this way, you don't need to remember the order of actual and expected arguments to functions like `assert.equal`, which helps you write better tests.

## Installation

Using [npm](https://www.npmjs.org/):

    $ npm install expect

Then with a module bundler like webpack, use as you would anything else:

```js
// using an ES6 transpiler, like babel
import expect, { createSpy, spyOn, isSpy } from 'expect'

// not using an ES6 transpiler
var expect = require('expect')
var createSpy = expect.createSpy
var spyOn = expect.spyOn
var isSpy = expect.isSpy
```

There is a UMD build in the npm package in the `umd` directory. Use it like:

```js
var expect = require('expect/umd/expect.min')
```

## Assertions

### toExist

> `expect(object).toExist([message])`

Asserts the given `object` is truthy.

```js
expect('something truthy').toExist()
```

#### toNotExist

> `expect(object).toNotExist([message])`

Asserts the given `object` is falsy.

```js
expect(null).toNotExist()
```

### toBe

> `expect(object).toBe(value, [message])`

Asserts that `object` is strictly equal to `value` using `===`.

### toNotBe

> `expect(object).toNotBe(value, [message])`

Asserts that `object` is not strictly equal to `value` using `===`.

### toEqual

> `expect(object).toEqual(value, [message])`

Asserts that the given `object` equals `value` using [deep-equal](https://www.npmjs.com/package/deep-equal).

### toNotEqual

> `expect(object).toNotEqual(value, [message])`

Asserts that the given `object` is not equal to `value` using [deep-equal](https://www.npmjs.com/package/deep-equal).

### toThrow

> `expect(block).toThrow([error], [message])`

Asserts that the given `block` `throw`s an error. The `error` argument may be a constructor (to test using `instanceof`), or a string/`RegExp` to test against `error.message`.

```js
expect(function () {
  throw new Error('boom!')
}).toThrow(/boom/)
```

### withArgs

> `expect(block).withArgs(...args).toThrow([error], [message])`

Asserts that the given `block` `throw`s an error when called with `args`. The `error` argument may be a constructor (to test using `instanceof`), or a string/`RegExp` to test against `error.message`.

```js
expect(function (check) {
  if (check === 'bad')
    throw new Error('boom!')
}).withArgs('bad').toThrow(/boom/)
```

### withContext

> `expect(block).withContext(context).toThrow([error], [message])`

Asserts that the given `block` `throw`s an error when called in the given `context`. The `error` argument may be a constructor (to test using `instanceof`), or a string/`RegExp` to test against `error.message`.

```js
expect(function () {
  if (this.check === 'bad')
    throw new Error('boom!')
}).withContext({ check: 'bad' }).toThrow(/boom/)
```

### toNotThrow

> `expect(block).toNotThrow([message])`

Asserts that the given `block` does not `throw`.

### toBeA(constructor)

> `expect(object).toBeA(constructor, [message])`<br>
> `expect(object).toBeAn(constructor, [message])`

Asserts the given `object` is an `instanceof constructor`.

```js
expect(new User).toBeA(User)
expect(new Asset).toBeAn(Asset)
```

### toBeA(string)

> `expect(object).toBeA(string, [message])`<br>
> `expect(object).toBeAn(string, [message])`

Asserts the `typeof` the given `object` is `string`.

```js
expect(2).toBeA('number')
```

### toNotBeA(constructor)

> `expect(object).toNotBeA(constructor, [message])`<br>
> `expect(object).toNotBeAn(constructor, [message])`

Asserts the given `object` is *not* an `instanceof constructor`.

```js
expect(new User).toBeA(User)
expect(new Asset).toBeAn(Asset)
```

### toNotBeA(string)

> `expect(object).toNotBeA(string, [message])`<br>
> `expect(object).toNotBeAn(string, [message])`

Asserts the `typeof` the given `object` is *not* `string`.

```js
expect(2).toBeA('number')
```

### toMatch

> `expect(string).toMatch(pattern, [message])`

Asserts the given `string` matches `pattern`, which must be a `RegExp`.

```js
expect('a string').toMatch(/string/)
```

### toBeLessThan

> `expect(number).toBeLessThan(value, [message])`<br>
> `expect(number).toBeFewerThan(value, [message])`

Asserts the given `number` is less than `value`.

```js
expect(2).toBeLessThan(3)
```

### toBeGreaterThan

> `expect(number).toBeGreaterThan(value, [message])`<br>
> `expect(number).toBeMoreThan(value, [message])`

Asserts the given `number` is greater than `value`.

```js
expect(3).toBeGreaterThan(2)
```

### toInclude

> `expect(array).toInclude(value, [comparator], [message])`<br>
> `expect(array).toContain(value, [comparator], [message])`

Asserts the given `array` contains `value`. The `comparator` function, if given, should compare two objects and either `return false` or `throw` if they are not equal. It defaults to `assert.deepEqual`.

```js
expect([ 1, 2, 3 ]).toInclude(3)
```

### toExclude

> `expect(array).toExclude(value, [comparator], [message])`<br>
> `expect(array).toNotContain(value, [comparator], [message])`

Asserts the given `array` does not contain `value`. The `comparator` function, if given, should compare two objects and either `return false` or `throw` if they are not equal. It defaults to `assert.deepEqual`.

```js
expect([ 1, 2, 3 ]).toExclude(4)
```

### (string) toInclude

> `expect(string).toInclude(value, [message])`<br>
> `expect(string).toContain(value, [message])`

Asserts the given `string` contains `value`.

```js
expect('hello world').toInclude('world')
expect('hello world').toContain('world')
```

### (string) toExclude

> `expect(string).toExclude(value, [message])`<br>
> `expect(string).toNotContain(value, [message])`

Asserts the given `string` does not contain `value`.

```js
expect('hello world').toExclude('goodbye')
expect('hello world').toNotContain('goodbye')
```

## Chaining Assertions

Every assertion returns an `Expectation` object, so you can chain assertions together.

```js
expect(3.14)
  .toExist()
  .toBeLessThan(4)
  .toBeGreaterThan(3)
```

## Spies

expect also includes the ability to create spy functions that can track the calls that are made to other functions and make various assertions based on the arguments and context that were used.

```js
var video = {
  play: function () {},
  pause: function () {},
  rewind: function () {}
}

var spy = expect.spyOn(video, 'play')

video.play('some', 'args')

expect(spy.calls.length).toEqual(1)
expect(spy.calls[0].context).toBe(video)
expect(spy.calls[0].arguments).toEqual([ 'some', 'args' ])
expect(spy).toHaveBeenCalled()
expect(spy).toHaveBeenCalledWith('some', 'args')

spy.restore()
expect.restoreSpies()
```

### createSpy

> `expect.createSpy()`

Creates a spy function.

```js
var spy = expect.createSpy()
```

### spyOn

> `expect.spyOn(target, method)`

Replaces the `method` in `target` with a spy.

```js
var video = {
  play: function () {}
}

expect.spyOn(video, 'play')
video.play()

spy.restore()
```

### restoreSpies

> `expect.restoreSpies()`

Restores all spies created with `expect.spyOn()`. This is the same as calling `spy.restore()` on all spies created.

```js
// mocha.js example
beforeEach(function () {
  expect.spyOn(profile, 'load')
})

afterEach(function () {
  expect.restoreSpies()
})

it('works', function () {
  profile.load()
  expect(profile.load).toHaveBeenCalled()
})
```

## Spy methods

### andCall

> `spy.andCall(fn)`

Makes the spy invoke a function `fn` when called.

```
var dice = createSpy().andCall(function () {
  return (Math.random() * 6) | 0
})
```

### andCallThrough

> `spy.andCallThrough()`

Makes the spy call the original function it's spying on.

```js
spyOn(profile, 'load').andCallThrough()

var getEmail = createSpy(function () {
  return "hi@gmail.com"
}).andCallThrough()
```

### andReturn

> `spy.andReturn(object)`

Makes the spy return a value.

```js
var dice = expect.createSpy().andReturn(3)
```

### andThrow

> `spy.andThrow(error)`

Makes the spy throw an `error` when called.

```js
var failing = expect.createSpy()
  .andThrow(new Error('Not working'))
```

### restore

> `spy.restore()`

Restores a spy originally created with `expect.spyOn()`.

## Issues

Please file issues on the [issue tracker on GitHub](https://github.com/mjackson/expect/issues).

## Tests

To run the tests in node:

    $ npm install
    $ npm test

To run the tests in Chrome:

    $ npm install
    $ npm run test-browser

## License

[MIT](http://opensource.org/licenses/MIT)
