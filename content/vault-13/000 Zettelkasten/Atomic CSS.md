---
UID: "202402062236"
Links:
  - "[[CSS]]"
  - "[[Atomic CSS]]"
  - "[[Patterns]]"
---
# Atomic CSS

Tailwind. Tachyons. Atomic classes can reduce style bundles but what is the real trade off byte wise to markup? And does the cognitive overhead of that littered markup—is it worth the cost? Debugging also more difficult. 

I have mixed feelings about Tailwind. I like parts of the concept. I think "atomic css" is a good idea, and I think the way it gently nudges you into following a good color/type/sizing system with less play and fewer decisions to make is fantastic. The biggest downsides I see are that: 1) it isn't CSS, so you have to learn a different concept and the terminology isn't always the same; 2) it adds a build step that can just be a real pain in the ass sometimes; 3) it subverts the default style debugging model which is to say that it makes working on styles in the browser a hassle. The last one is really important to my workflow so it impacts me a lot but if you don't have that history it might not make a difference.

Tailwind also has a UI component library you can buy, which can help you scaffold out some of the common UI elements quickly.

Also, Tailwind has gotten _really_ popular so you see good support for it in surprising places. I think it would probably be a good option for you based on my guesses of what kind of things your making.

