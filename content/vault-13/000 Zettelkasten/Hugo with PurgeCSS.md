---
UID: "202402282127"
---
# Hugo with PurgeCSS

Links: [[CSS Minification]], [[PurgeCSS]], [[Hugo]]

I am looking at adding [Open Props](https://open-props.style)—a CSS "library" that consists of variables and a reset file—to Amanda's website.

It's not a huge dependency, but it at 4-6 KB *compressed* it isn't nothing. Given that I expect to only use very little of, that would be something like 85% waste which just feels bad. It looks like you can use PurgeCSS with Hugo, and that PurgeCSS has an option to strip unused CSS variables. So, that might do the trick.

I'll find out later, because if that doesn't work there's a library that takes the vars and strips them through PostCSS, called `postcss-jit-props`.

# References

https://purgecss.com/guides/hugo.html