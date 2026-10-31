Links:  [[CraftCMS]], [[CKEditor]], [[Embeds]]
Status: #bloggable

You can use CKEditor to embed things in Rich Text like videos from Vimeo and YouTube with just their links, and CKEditor will create some HTML that will render the video appropriately on your front end.

To get it working you have to add the following configuration options to CKEditor:

```json
{
	"mediaEmbed": {
		"previewsInData": true
	}
}
```

And then you also have to tell the HTML purifier not to strip out the iframes by expanding the regex used for the `URI.SafeIframeRegexp` parameter in your Craft's purifier config at `myproject/config/htmlpurifier/Default.json`.

> [!WARNING] Be careful with the escaped backslashes.
> Some code editors might double the backslashes in an effort to be helpful.

```json
{  
	...
	"URI.SafeIframeRegexp": "%^(https?:)?//(www\\.youtube\\.com/embed/|www\\.youtube\\.com/watch\\?v=.+|youtu\\.be/.+|www\\.youtube(?:-nocookie)?\\.com/embed/|player\\.vimeo\\.com/video/)%"  
}
```

And finally, you may also need to add some global CSS to fix the height of the embed. For some reason, at least on my instances, CKEditor's generated figured tries to used the [percentage-based padding trick](https://css-tricks.com/aspect-ratio-boxes/#aa-the-core-concept-padding-in-percentages-is-based-on-width) to set the aspect ratio, but doesn't set the necessary position properties. It does add a media class, so we can do this: 

```css
/* Fix positioning for OEmbeds in Rich Text */
.media div:has(> iframe) {
	position: relative;
}

.media iframe {
	position: absolute;
}
```

The bit that says `div:has(> iframe)` lets us get the direct parent of the iframe, which is where the padding will be set. The very cool [has selector](https://developer.mozilla.org/en-US/docs/Web/CSS/:has) finally available in all browsers and I thought this might be a little more resilient.

I think the fact that one needs to do all this means I'm either doing something wrong or this is a bug, so I'd like to figure that out sometime and maybe submit some PRs if it is indeed a bug.