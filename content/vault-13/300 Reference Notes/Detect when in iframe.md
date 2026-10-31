[[<iframe>]], [[frameElement]]

I am not aware of a way to do this with CSS but that would be great.

In JS you can compare `window.self` to `window.top`.

```js
function inIframe() {
	try {
		return window.self !== window.top;
	} catch (e) {
		return true;
	}
}
```


You can check if there is a frame element with [`window.frameElement`](https://developer.mozilla.org/en-US/docs/Web/API/Window/frameElement) This doesn't seem to work though? Didn't work for me in initial test at least.

```js
// note that this is silly code meant to convey concept, use a singleton
function inIframe() {
	return !!window.frameElement
}
```

