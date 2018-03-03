I needed to interact with the browser's [History](https://developer.mozilla.org/en-US/docs/Web/API/History) object, but it was too mind-bending.  So I made this abstraction and then I was able to get my job done.

These functions update and return state using a single property on the real state object, minimizing your odds of bashing history state set by some other part of your app when you use this API.

# API

```js
const historyState = require('better-history-api')
```

## `historyState.update(newState)`

Updates the existing state for the current history item.

The new state object you pass in is merged with the existing state with `Object.assign`.

## `state = historyState.get()`

Returns the state object for the current history item.

## `historyState.addBeforePushStateMiddleware(fn)`

This adds a callback/middlewarey function that will be called *before* `pushState` is called, no matter who is calling `pushState`.

Your function will be passed the current (pre-`pushState`) state, and will give you a chance to change it before the history stack changes.

```js
historyState.addBeforePushStateMiddleware(
	state => Object.assign(state, { position: currentScrollPosition() })
)
```

## Events

The `historyState` object is an [event emitter](https://github.com/TehShrike/better-emitter) that emits these events:

### `new state`

Emitted immediately after `pushState` is called, while the state object is fresh and shiny.

### `old state`

Emitted whenever a `popstate` event happens with any associated state.  Emits the state object.

# License

[WTFPL](http://wtfpl2.com)
