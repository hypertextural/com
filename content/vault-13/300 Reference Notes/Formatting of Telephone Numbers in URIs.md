[[URI]]

The official specification for telephone number URIs allows for visual separators **excluding spaces**.

So the following characters are fine: `- . ( )`

That’s a standard dash, a dot, and open/close parentheses.

I sometime advise sticking to this format: `18283748706`. That’s because implementations don’t necessarily follow the spec, and it was the case many years ago that the separator-less version could be more reliable. That probably isn’t the case today.