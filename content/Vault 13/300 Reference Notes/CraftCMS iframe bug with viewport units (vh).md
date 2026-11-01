iframes set a new viewport context, so `vh` units don’t work like you expect. Then Craft sets the height of the iframe dynamically based on how big the page is. It gets into a loop and grows very tall.

see [[V* in IFrames]]

Links: [[CraftCMS]], [[Viewport Units]], [[<iframe>]]