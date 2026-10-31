---
Links:
  - "[[Web Components]]"
  - "[[Dialogs]]"
---
## Research

- [MDN Dialog Element Docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog)
	- Use `HTMLDialogElement.showModal()` to open, don't set the `open` attribute.
		- There's also a `.show()` method for use with non-modal dialogs.
	- Close the modal with `HTMLDialogElement.close()`.
	- If the `<dialog>` contains a `<form method="dialog">` then submitting the form closes the modal.
		- A button with `formmethod="dialog"` also works.
		- You also get a return value on `HTMLDialogElement.returnValue`.
	- Use the `::backdrop` pseudo-element to style the modal backdrop.
	- You should `autofocus` your first input if you are making a form.
		- e.g., `<button autofocus …>`
	- Dialog handles `esc` for you! :-)
- [Polypane](https://polypane.app/blog/building-a-lightbox-with-the-dialog-element/)
	- This is mostly just some DOM manipulation round injecting some dialogs for documentation refs in markdown. Not really relevant to what I'm doing.
- [Raymond Camden: Image Dialog Web Component](https://www.raymondcamden.com/2023/12/13/an-image-dialog-web-component)
	- He uses an `<a><img/></a>` structure as the fallback to the component.
		- This makes sense to me, it's a good fallback, but I wonder if falling back to the dialog itself is fine.
	- His custom element inserts all the form stuff via JS, `innerHTML`.
	- Uses a blur for the `::backdrop`.
- [Web Dev: Building a Dialog Component](https://web.dev/articles/building/a-dialog-component)
	- *Authored by* **Adam Argyle**
	- Dated 2022-04-13.
	- [Source on GH](https://github.com/argyleink/gui-challenges/tree/main/dialog)
	- Best practice to auto focus the cancel button.
		- See [[Gallery Modal Web Component#Autofocus the close|Autofocus close note]].
		- But also note that browser will focus the first focusable element *for you*.
	- Mentioned [inert](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/inert), and that the `<dialog>` helpfully manages it for you. 
	- Styles closed like in [[Gallery Modal Web Component#Styling the closed modal.|note on styling closed modal]].
		- Will add some JS to manage `inert` and ensure screen-readers & keyboard users can't accidentally access closed modal.
	- Makes use of `<header>`, `<footer>`, and `<article>` elements to section the dialog itself, **which I think is neat**.
	- Uses `max-inline-size` and `max-block-size` which is pretty interesting, and uses the `dvb` unit.
		- See [[Gallery Modal Web Component#Responsive Dialog Size|Responsive Dialog Size]]
	- Centers everything with `margin: auto`, `position: fixed` and `inset: 0`.
		- Media query pushes modal to bottom on mobile, closer to thumbs.
	- Centers small modals over the originating element [by getting bounds of the closest opener element](https://github.com/argyleink/gui-challenges/blob/main/dialog/index.js#L92-L102).
	- Styled the `::backdrop` with a `backdrop-filter: blur(25px)` and added a transition in the hopes it gets supported in the future.
		- The idea of targeting possible future implementations is a good one, "hopeful" or "optimistic" style info.
	- Uses the now supported `:has()` property to kill BG scroll.
	- Styling the form itself, they use the same `max-block-size` values again, and use a `grid-template-rows: auto 1fr auto`, for the `heading`, `article`, and `footer` respectively.
	- They use `padding-block` and `padding-inline` values--something I need to look into.
		- [x] Read about these!
		- but do people who read in other directions actually have the same expectations of block and inline size we do? i.e. is 60ch in vertical Japanese actually a reasonable height?e
		- these are those left/right top/bottom switches aren't they?
	- [x] are `ch` relative to document font size or element font size?
	- uses `place-items` and `stroke` to style the button
		- [ ] read about these, too
	- use `overscroll-behavior` so that you don't get that annoying over-scroll jiggle.
	- `menu` element for buttons! excellent, love this!
	- They add some nice animation bits.
	- Triggers dismiss when onclick matches DIALOG nodeName
	- Uses some events to handle & track closing & closed
		- [[Gallery Modal Web Component#Tracking the Dialog Close|See Tracking Close Code Snippet]].
	- Using a mutation observer for tracking open
		- See [[Gallery Modal Web Component#Tracking Open State with Mutation Observer|Mutation Observer Snippet]].
	- Use another observer to [[Gallery Modal Web Component#Clean Up Events|clean up event handlers]].
	- Uses a `loading` attribute to prevent animating on page load, attribute gets removed by JS after awaiting `animationsComplete()`

### Design Pattern Notes

- The Mutation Observer and the open/close stuff is a good pattern for any element you need to show/hide.

### Accessibility Notes

> Automatically focusing a form control can confuse visually-impaired people using screen-reading technology and people with cognitive impairments. When `autofocus` is assigned, screen-readers "teleport" their user to the form control without warning them beforehand.

>Use careful consideration for accessibility when applying the `autofocus` attribute. Automatically focusing on a control can cause the page to scroll on load. The focus can also cause dynamic keyboards to display on some touch devices. While a screen reader will announce the label of the form control receiving focus, the screen reader will not announce anything before the label, and the sighted user on a small device will equally miss the context created by the preceding content.

From [MDN's notes on the `autofocus` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/autofocus).

### Interesting Side Info

- [CSS color-scheme property](https://developer.mozilla.org/en-US/docs/Web/CSS/color-scheme)
- [Experimental light-dark CSS color function](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/light-dark)
- [Chrome's Auto Dark Theme](https://developer.chrome.com/blog/auto-dark-theme/#per-element-opt-out)
- [Web.dev Deep Dive on Color Schemes](https://web.dev/articles/building/a-color-scheme)
- [Open Props: CSS variable library](https://open-props.style/)
	- This might provide the actually *good* stuff that Tailwind does but without… you know… using Tailwind.
- https://www.mayank.co/blog/custom-element-base
- I really need to review all of CSS's new units. 
	- [Container query length units](https://developer.mozilla.org/en-US/docs/Web/CSS/length#container_query_length_units)
	- [vb and vi containing block percentage units](https://developer.mozilla.org/en-US/docs/Web/CSS/length#vb)
	- [Viewport units cont'd](https://developer.mozilla.org/en-US/docs/Web/CSS/length#relative_length_units_based_on_viewport)
		- `sv*`, `lv*`, and `dv*` are all new and interesting.
- **Mayank's very interesting article about exciting DOM functions with Proxy and Reflect to [create a custom element base class](https://www.mayank.co/blog/custom-element-base/).**

### Autofocus the Close Button

```html
<button autofocus type="reset" onclick="this.closest('dialog').close('cancel')">
	Button
<button>
``` 

### Styling the Closed Modal

```css
dialog:not([open]) {
	pointer-envents: none;
	opacity: 0;
}
```

### Responsive Dialog Size

```css
dialog {
	…
	/* 60ch is readable width, but don't expand edge to edge */
	max-inline-size: min(90vw, 60ch);

	/* don't exceed height */
	max-block-size: min(80vh, 100%);

	/* actual goal is to keep within _relative_ flow, dvb handles that,
	   but will fall back to vh if not supported by browser yet */
	max-block-size: min(80dvb, 100%);
	
	overflow: hidden;
}
```

### Kill Background Scrolling

```css
html:has(dialog[open][my-dialog-type]) {
	overflow: hidden;
}
```

### Tracking the Dialog Close

```js
const dialogClosingEvent = new Event('closing');
const dialogClosedEvent = new Event('closed');

export default async function(dialog) {
	…
	dialog.addEventListener('close', dialogClose);
}

const dialogClose = async ({ target: dialog }) => {
	dialog.setAttribute('inert', '');
	dailot.dispatchEvent(dialogClosingEvent);
	
	await animationsComplete(dialog);

	dialog.dispatchEvent(dialogClosedEvent);
}

const animationsComplete = element => Promise.allSettled(
	element.getAnimations().map(animation => animation.finished)
);
```

### Tracking Open State with Mutation Observer

```js
const dialogOpeningEvent = new Event('opening');
const dialogOpenedEvent = new Event('opened');

export default async function(dialog) {
	…
	dialogAttrObserver.observe(dialog, { attributes: true });
}

const dialogAttrObserver = new MutationObserver((mutations, observer) => {
	mutations.forEach(async mutation => {
		if (mutation.attributeName === 'open') {
			const dialog = mutation.target;
			const isOpen = dialog.hasAttribute('open');

			if (!isOpen) return;

			dialog.removeAttribute('inert');

			const focusTarget = dialog.querySelector('[autofocus]');
			focusTarget 
				? focusTarget.focus()
				: dialog.querySelector('button').focus();

			dialog.dispatchEvent(dialogOpeningEvent);
			await animationsComplete(dialog);
			dialog.dispatchEvent(dialogOpenedEvent)
		}
	})
})
```

### Clean Up Events

```js
const dialogRemovedEvent = new Event('removed');

export default async function(dialog) {
	…
	dlgDelObserver.observe(document.body, {
		attributes: false,
		subtree: false,
		childList true
	})
}

const dlgDelObserver = new MutationObserver((mutations, observer) => {
	mutations.forEach(mutation => {
		mutation.removedNodes.forEach(removedNode => {
			if (removedNode.nodeName === 'DIALOG') {
				removedNode.removeEventListener('click', lightDismiss);
				removedNode.removeEventListener('close', dialogClose);
				removedNode.dispatchEvent(dialogRemovedEvent);
			}
		})
	})
})
```

## Implementation

### Notes for Hugo

I'm doing this in Hugo so I had to do some extra stuff to get things basically set up, i.e., "build" the JS.

I started with a `main.js` that was imported into a partial at `./layouts/partials/head/js.html`, it looked like this:

```html
{{- with resources.Get "js/main.js" }}  
  {{ if eq hugo.Environment "development" }}  
   <script type="module" src="{{ .RelPermalink }}"></script>  
  {{- else }}  
   {{- with . | minify | fingerprint }}  
    <script type="module" src="{{ .RelPermalink }}"  
        integrity="{{ .Date.Integrity }}"  
        crossorigin="anonymous"  
    ></script>  
   {{- end }}  
  {{- end }}  
{{- end }}
```

and it was imported into my head partial in the usual way (e.g., `{{ partialCached "head/js.html" . }}`). In order to get builds working so that I could `import` scripts without having to bother with extra actual `<script>` tags, all I had to do was add  a [`js.Build` Hugo Pipe](https://gohugo.io/hugo-pipes/js/) to my `resources.Get`, and have my `main.js` serve as the root entry point. I also added inline source maps so debugging would be easier. It now looks like this:

```html
{{- with resources.Get "js/main.js" | js.Build (dict "sourceMap" "inline") }}  
  {{ if eq hugo.Environment "development" }}  
   <script type="module" src="{{ .RelPermalink }}"></script>  
  {{- else }}  
   {{- with . | minify | fingerprint }}  
    <script type="module" src="{{ .RelPermalink }}"  
        integrity="{{ .Date.Integrity }}"  
        crossorigin="anonymous"  
    ></script>  
   {{- end }}  
  {{- end }}  
{{- end }}
```

and my `main.js` simply has an import line for my module now (file path is relative to `main.js`):

```js
import galleryDialog from "./modules/GalleryDialog/GalleryDialog";
```

### Creating the Fallback

I used the idea from [[Gallery Modal Web Component#Research|GIVE ME TITLE, FIX LINK]] and created my fallback markup so that if JS were broken for whatever reason the component will still function by opening the full size image through a regular link. The undefined element will just be treated like a plain `<div>` until JS defines it.

```html
<gallery-dialog>  
  <a href="{{ full-size-url }}">  
    <img class="image" alt="" src="{{ preview-size-url }}"/>  
  </a>  
</gallery-dialog>
```

### Creating the Base Web Component

To start, I want to create the dialog, pull some attributes from the fallback html, create some buttons and put it all in place within the component. I'm going to create the elements by way of public fields, though, as you'll see later, this will start to grow and I think I'll want to come up with a more declarative / HTML-y approach.

```ts
export default class GalleryDialog extends HTMLElement {
	$dialog = document.createElement('dialog');
	$bigImg = document.createElement('img');
	$openBtn = document.createElement('button');
}

// don't forget to define the element!
customElements.define('gallery-dialog', GalleryDialog);
```

I'm also using TypeScript, which I may soon come to regret and dump if it gets too annoying--anyway, it means that I'm going to define some private class properties and lazy getters like so:

```ts
export default class GalleryDialog extends HTMLElement {
	...
	
	private _thumb: HTMLImageElement | null = null;
	private _anchor: HTMLAnchorElement | null = null;
	private _alt: string | null = null;
	private _bigUrl: string | null = null;

	// find our thumbnail element
	get $thumb(): HTMLImageElement {
		this._thumb ||= this.querySelector('img');
		if (!this._thumb) {
			throw new GDError('`img` thumbnail child required, none found');
		}
		return this._thumb;
	}

	// find the fallback anchor
	get $anchor(): HTMLAnchorElement {
		this._anchor ||= this.querySelector('a');
		if (!this._anchor) {
			throw new GDError('`a` child required, none found');
		}
		return this._anchor;
	}

	// extract alt text from the thumbnail
	get alt(): string {
		this._alt ||= this.$thumb.getAttribute('alt');
		if (!this._alt) {
			throw new GDError('`img` thumbnail missing alt');
		}
		return this._alt;
	}

	// extract full size image URL from the fallback anchor
	get bigUrl(): string {
		this._bigUrl ||= this.$anchor.getAttribute('href');
		if (!this._bigUrl) {
			throw new GDError('`a` missing href for full size image URL');
		}
		return this._bigUrl;
	}

	...
}
```

I'm not crazy about this pattern, it's more than a little verbose and repetitive, but I think it's functionally helpful in at least a couple ways:

1. The fact that I can always count on these properties being defined makes them much less annoying to use later, and
2. It gives me a very convenient check point to throw errors if the component is misconfigured, potentially coming in handy later if I want to add more complex validation.

In the `constructor` is the basic logic to place the elements, set attributes, and bind listeners.

```ts
export default class GalleryDialog extends HTMLElement {
	...

	constructor() {
		// class properties get instantiated here
		super()

		// add the thumbnail to our new dialog opening button
		// and replace the fallback anchor with the button
		this.$openBtn.appendChild(this.$thumb);
		this.$anchor.replaceWith(this.$openBtn);

		// set the full size image with extracted alt and src
		this.$bigImg.setAttribute('alt', this.alt);
		this.$bigImg.setAttribute('src', this.bigUrl);

		// add the image to our dialog, add the dialog to
		// the component
		this.$dialog.appendChild(this.$bigImg);
		this.appendChild(this.$dialog);

		// bind a 'click' event listener to `this`
		this.addEventListener('click', this);
	}

	...
}
```

Something noteworthy and neat about the above is this line:

```ts
// bind a 'click' event listener to `this`
this.addEventListener('click', this);
```

That works because our class implements the `handleEvent` method, like so:

```ts
export default class GalleryDialog extends HTMLElement {
	...

	handleEvent(event: Event) {
		switch (event.type) {
			case 'click':
				return this.handleClick(event as MouseEvent);
			default:
				console.warn(`Event ${event.type} triggered without handler.`);
		}
	}

	...
}
```

The above pattern is something I learned reading Chris Ferdinandi's article on [using the handleEvent() method in Web Components](https://gomakethings.com/the-handleevent-method-is-the-absolute-best-way-to-handle-events-in-web-components/). There are several upsides to this:

1. You don't have to `bind` or `() => myHandler()` your event handlers, the `this` context will be preserved.
2. The handler event listeners are removed if your class component is removed, so you get easy cleanup.
3. Developer experience aside, this method uses *substantially* less memory than bound or arrow-functioned listeners.

Andrea Giammarchi goes into detail on the [performance benefits of handleEvent](https://webreflection.scribe.rip/dom-handleevent-a-cross-platform-standard-since-year-2000-5bf17287fd38) in his article of the same topic. He also cites this neat pattern to directing the events that is much more succinct (if you don't have a type linter yelling at you or you feel like fiddling with it enough to make it stop):

```js
class SomeClass {
	constructor() {
		this.addEventListener('click', this);
	}
	
	handleEvent(event) {
		this['on' + event.type](event);
	}
	
	onclick(event) {
		event.preventDefault();
		console.log(event);
	}
}
```

Back to the dialog! Our `handleClick` is pretty simple right now. We simply trigger the [dialog element's native `showModal` method](https://developer.mozilla.org/en-US/docs/Web/API/HTMLDialogElement/showModal) to open it.

```ts
handleClick(event: MouseEvent) {
	this.$dialog.showModal();
}
```

That's enough to get the dialog opening! Because we're using the native `<dialog>` element, we even keyboard support (close on `esc`) and backdrop `inert`-ing for free.

I have added a few styles at this point, as well:

```css
/* stop scroll when dialog is open */
html:has(dialog[open]) {  
  overflow: hidden;  
}  
  
gallery-dialog {  
	display: flex;  
	
	/* there's some contention about what elements should or should
	   not have `pointer` cursors, but I think in this case the
	   additional affordance makes sense */
	& button {  
		cursor: pointer;  
	}  
	
	& dialog {  
		& img {  
			/* based on my common img size */  
			max-width: min(85vw, 1600px);  
			max-height: min(85vh, 1600px);  
			object-fit: contain;  
		}  
	}  
}  

/* just a little shadow and blur on the backdrop */
dialog::backdrop {  
	background-color: rgba(0, 0, 0, 0.2);  
	-webkit-backdrop-filter: blur(2px);  
	backdrop-filter: blur(2px);  
}
```

### Adding Navigational Buttons

Closing the dialog with `esc` is nice, but it should really have a close button, and I think it would be good if it had a pair of buttons to navigate to the next or previous image, too.

I'll start by adding the buttons themselves.

```ts
export default class GalleryDialog extends HTMLElement {
	...

	$closeBtn = document.createElement('button');
	$prevBtn = document.createElement('button');
	$nextBtn = document.createElement('button');

	...
	
	constructor() {
		...

		this.$closeBtn.append('Close');  
		this.$closeBtn.setAttribute('autofocus', '');  
		  
		this.$prevBtn.append('Previous');  
		this.$nextBtn.append('Next');  

		// append elements to $dialog in block
		this.$dialog.append(  
			this.$bigImg,  
			this.$closeBtn,  
			this.$prevBtn,  
			this.$nextBtn  
		);

		...
	}

	...
}
```

Notice the `autofocus` attribute on `$closeBtn`: this tells the browser to focus that button when the dialog opens. This is good for accessibility because a keyboard user will be able to easily close the modal after opening it.

I also need to add some additional click handlers. Again, this is pretty verbose and I think I'll end up simplifying.

```ts
export default class GalleryDialog extends HTMLElement {
	...

	handleClick(event: MouseEvent) {  
		// brittle, but we'll adjust later
		const button = (event.target as HTMLButtonElement).closest('button');
	
		switch (button) {  
			case this.$openBtn:  
				return this.showModal();  
			case this.$closeBtn:  
				return this.close();  
			case this.$prevBtn:  
				return this.goToPrev();  
			case this.$nextBtn:  
				return this.goToNext();  
			default:  
				console.warn(`Unhandled click event for button: ${button}`);  
		}  
	}

	// convenience method to reach $dialog method
	close() {
		return this.$dialog.close();
	}

	// convenience method to reach $dialog method
	showModal() {
		return this.$dialog.showModal();
	}

	goToPrev() {
		if (this.previousElementSibling instanceof GalleryDialog) {
			this.close();
			this.previousElementSibling.showModal();
		}
	}

	goToNext() {
		if (this.nextElementSibling instanceof GalleryDialog) {
			this.close();
			this.nextElementSibling.showModal();
		}
	}

	...
}
```

I really like how these `goToPrev/Next` functions come together. We see if the `nextElementSibling` is another one of our GalleryDialog instances, and if it is, we can simply close our current instance and tell that next instance to display. 

### Add Keyboard Navigation

It's quite straightforward to add some ← and → navigational keyboard handlers now.

```ts
export default class GalleryDialog extends HTMLElement {
	...

	constructor() {
		...
		this.addEventListener('keydown', this);
	}

	handleEvent(event: Event) {
		switch (event.type) {
			case 'click':
				return this.handleClick(event as MouseEvent);
			// add a case for 'keydown'
			case 'keydown':
				return this.handleKeyDown(event as KeyboardEvent);
			default:
				console.warn(`Event ${event.type} triggered without handler.`);
		}
	}

	handleKeyDown(event: KeyboardEvent) {
		switch (event.key) {
			case 'ArrowLeft':
				return this.goToPrev();
			case 'ArrowRight':
				return this.goToNext();
		}
	}

	...
}
```

### Closing the Dialog On Backdrop Click

Closing the dialog when you click on the backdrop is a little trickier than might be initially apparent because the backdrop is really `::backdrop`, a pseudo element, so it's clicks are targeted as the `dialog` itself. This is fine, however, if the dialog's actual content exists in a padding-less container such that any click outside that container is essentially a backdrop click (i.e., a single parent `form` or `div` element). Otherwise you could check the `dialog`'s bounding rectangle, but I would prefer not to. So let's just close when a click is received on `dialog`.

```ts
handleClick(event: MouseEvent) {
	// guard for TS convenience
	if (!event.target || !(event.target instanceof HTMLElement)) {
		return;
	}

	// if the target is our dialog, close
	if (event.target.nodeName === 'DIALOG') {
		return this.close();
	}

	...
}
```

### Refactoring

As I mentioned earlier, in a few cases I wasn't entirely happy with how the `GalleryDialog` code was working with the DOM. It felt very procedural, imperative, and repetitive. I also wanted to start adding a lot more structure to the `<dialog>` element itself. 

And so I set out in search of a Better Way. Follow along if you'd like a story, otherwise get scrolling—if you pass the giant code block you've gone too far, if you pass the 7/11 you… have uh… also gone too far.

One of the biggest pains I was experiencing related to all my "constructor junk", i.e. the calls to `document.createElement()` over and over, followed by calls to `someElement.setAttribute()` and friends. So I thought maybe I should try setting `innerHTML` or using `insertAdjacentHTML()` to create chunks of HTML in one go, like so:

```js
someElement.insertAdjacentHTML(`
	<header>
		<h2>${this.title}</h2>
	</header>
`);
```

It's an abbreviated example but it should still serve to make my point: with these methods/setters I could write my HTML more… like actual HTML, but it didn't really save my from having to create the attachment points and at times it got a little unwieldy.

I also thought about using [tagged templates](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#tagged_templates), like:

```js
const someHtml = html`<header><h2>${this.title}</h2></header>`;
```

I do like that, ergonomically, at least. This is how [Lit handles the problem](https://lit.dev/docs/templates/overview/). I wasn't sure I wanted to put the work into making the tag function just for this dialog component, though.

However, while I was thinking about tagged templates and where I might stash the DOM elements I was generating I got to reading about [`DocumentFragment`](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment) which lead to reading about [`<template>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template), which in turn lead to reading about [Declarative Shadow DOM] and [`<slot>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/slot)'s which got me pretty excited until I realized I would _have_ to use Shadow DOM to use slots which I did not want to do because this was supposed to be a simple little dialog and I just didn't want to encapsulate the styles and have to worry about the Shadoweirds while making this little thing.

So `<template>`'s, I thought, could be useful. I could even write the `<template>` in regular HTML then pop it up with a query selector and clone it away. Putting the HTML code where the HTML goes *is* a delightful thought, and that I might not have to repeat myself even more so.

And yet… where am I going to stick this `<template>`? Surely one doesn't just copy the same template into each instance of their component. Should I just put it a level up the the element hierarchy? Should I put it near the end of the `<body>`? You can't really just ship the HTML template along with the component code in a nice neat little way and there are no HTML Imports so…? I looked around to see if there were any common patterns for this. It looked like some people were just writing the template in JavaScript using one of the earlier methods I was looking at. Some other people were using `fetch` to grab the template with an extra HTTP request (oh my!). 

That all felt bad. While I was thinking about this I was also reading the [Declarative Shadow Dom proposal](https://github.com/mfreed7/declarative-shadow-dom/tree/master), and after looking at the [section on repetition](https://github.com/mfreed7/declarative-shadow-dom/tree/master#isnt-it-tedious-to-repeat-the-contents-of-each-shadow-root) it did finally occur to me that, yes, proposal writer Mason Freed, I *was* "missing the point entirely". I don't complain when I have write an `<li>` tag every time I want to render a list item (at least, definitely not when I'm writing my HTML through a Static Site Generator that handles duplication for me, *which I am*). I blame this block in thinking on JS frameworks—writing Web Components is not writing JS framework components but it *feels* kind of similar so it's easy to find yourself reasoning about them within the wrong paradigms.

So creating a bunch of duplicate copies of my template it is! Except, even better than that, I realized I only really needed a tiny fraction of my HTML to be in a detached, ready for cloning kind of state: the button I replace my fallback HTML with. Everything else, the `<dialog>` itself and all its descendants, can just be regular HTML! This all feels much better, is much easier to manage and cuts my JavaScript by about 45% (as measured by our favorite, definitely not very unreliable metric: line count).

Here's what my Hugo template looks like now:

```html
<gallery-dialog>
	<a href="{{ .src }}">
		{{ with $resized }}
			<img class="image" alt="TODO: front matter" src="{{ .RelPermalink }}"/>
		{{ end }}
	</a>
	<template>
		<button class="open" data-click="showModal">
			{{ with $resized }}
				<img class="image" alt="TODO: front matter" src="{{ .RelPermalink }}"/>
			{{ end }}
		</button>
	</template>
	<dialog>
		<div class="container">
			<header><h3>{{ .rsc.Title }}</h3></header>
			<img loading="lazy" alt="TODO: front matter" src="{{ .src }}"/>
			<footer>
				<menu>
					<li>
						<button autofocus data-click="close">Close</button>
					</li>
				</menu>
				<menu>
					<li>
						<button data-click="goToPrev">Previous</button>
					</li>
					<li>
						<button data-click="goToNext">Next</button>
					</li>
				</menu>
			</footer>
		</div>
	</dialog>
</gallery-dialog>

```

I used a `data-click` attribute to keep track of what elements should call what functions. I landed on `data-*` as the syntax because it feels fairly standard and I can use `HTMLElement.dataset` to access it simply. 

```ts
class GDError extends Error {
	constructor(message: string | undefined) {
		super(message);
		this.name = 'GalleryDialogError';
	}
}

type GDKey = keyof GalleryDialog;

export default class GalleryDialog extends HTMLElement {
	$dialog;

	constructor() {
		super();

		this.$dialog = this.strongQuerySelector('dialog');

		const anchor = this.strongQuerySelector('a', 'fallback anchor');
		const template = this.strongQuerySelector('template');

		anchor.replaceWith(template.content.cloneNode(true));

		this.addEventListener('click', this);
		this.addEventListener('keydown', this);
	}

	// I consolidated the code for erroring when the element was unfound.
	// The TS stuff is… contentious to me, but in this case I do really
    // like that I get some nice completions and checks based on what
    // kind of element I'm requesting from the function (e.g., 'dialog'
    // will inform TS that I'm definitely getting HTMLDialogElement back
    // from this function).
	strongQuerySelector<K extends keyof HTMLElementTagNameMap>(
		selectors: K,
		errorMsg?: string
	): HTMLElementTagNameMap[K] {
		let e = this.querySelector(selectors);
		if (!e) throw new GDError(`missing element "${errorMsg || selectors}"`);
		return e;
	}

	// noinspection JSUnusedGlobalSymbols
	handleEvent(event: Event) {
		switch (event.type) {
			case 'click':
				return this.handleClick(event as MouseEvent);
			case 'keydown':
				return this.handleKeyDown(event as KeyboardEvent);
			default:
				throw new GDError(`Event ${event.type} triggered without handler.`);
		}
	}

	// Seemed right to have a list of allowable function calls
	private clickFns = new Set<GDKey>([
		'close',
		'showModal',
		'goToPrev',
		'goToNext'
	]);

	handleClick(event: MouseEvent) {
		if (!event.target || !(event.target instanceof HTMLElement)) return;

		// not loving this weird case but it's fine.
		if (event.target.nodeName === 'DIALOG') return this.close();

		// I find the closest element with a click handler
		const e = event.target.closest('[data-click]');

		// get the string 'data-click' attribute
		const k = e && e instanceof HTMLElement && (e.dataset.click as GDKey);

		// make sure it's in my approved set, and then a bunch of
		// crap that I know isn't necessary and makes me really
		// annoyed with typescript even though I can see it kind of has
		// a point. There is probably a better way.
		if (!k || !this.clickFns.has(k) || typeof this[k] !== 'function')) {
			return console.warn(`tried to call unknown click handler "${k}"`);
		}

		// the 'as Function' 🤦🏻‍♂️
		(this[k] as Function)(event);
	}

	handleKeyDown(event: KeyboardEvent) {
		switch (event.key) {
			case 'ArrowLeft':
				return this.goToPrev();
			case 'ArrowRight':
				return this.goToNext();
		}
	}

	close() {
		return this.$dialog.close();
	}

	showModal() {
		return this.$dialog.showModal();
	}

	goToPrev() {
		if (this.previousElementSibling instanceof GalleryDialog) {
			this.close();
			this.previousElementSibling.showModal();
		}
	}

	goToNext() {
		if (this.nextElementSibling instanceof GalleryDialog) {
			this.close();
			this.nextElementSibling.showModal();
		}
	}
}

customElements.define('gallery-dialog', GalleryDialog);

```

I feel much better about this with the exception of the awkward typing. Now my `<dialog>`'s are in real HTML and I can just mark them up with nice semantics and regular CSS, which is what I'll do next.

### Styling!



```ts
export default class GalleryDialog extends HTMLElement {
	...

	

	...
}
```