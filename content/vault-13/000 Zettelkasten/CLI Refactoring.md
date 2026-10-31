---
UID: "202403121515"
---
# CLI Refactoring

Links: [[CLI Tools]], [[Refactoring]], [[Substitution]]

Fastmod is a useful tool for doing complex find & replace refactors from the command line. See the following example:

```
fastmod -m -d /home/jrosenstein/www --extensions php,html \
    '<font *color="?(.*?)"?>(.*?)</font>' \
    '<span style="color: ${1};">${2}</span>'
```

# References

https://github.com/facebookincubator/fastmod