---
UID: "202403181643"
---
# Supporting Hugo & Liquid in Helix

Links: [[Tree Sitter]], [[LSP]], [[Hugo]], [[Liquid]]

The main thing is that I need to create a parser that differentiates _code_ from _content_.

A good example is the existing Tree Sitter grammars for EJS/ERB Embedded Templates.

See the ERB grammar here: https://github.com/tree-sitter/tree-sitter-embedded-template/blob/master/grammar.js

And it's helpful to review the corpus, here: https://github.com/tree-sitter/tree-sitter-embedded-template/blob/master/corpus/main.txt

These are Helix's injection settings for ERB: https://github.com/helix-editor/helix/blob/master/runtime/queries/erb/injections.scm.

Here's how they handle the highlighting (though this is easier for ERB/EJS because the code is just Ruby/JS, so they don't need to do any highlighting for the actual code): https://github.com/helix-editor/helix/blob/master/runtime/queries/ejs/highlights.scm

With Hugo, the highlight won't be bad because I can just pass it back to GoTmpl, but for Liquid I don't think a highlighter exists.