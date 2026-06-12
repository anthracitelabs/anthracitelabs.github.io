---
layout: default
---


execute ctags to generate tags for you project. After this step <kbd>Ctrl</kbd> + <kbd>]</kbd> will go to definition.
<kbd>g</kbd> then <kbd>Ctrl</kbd> + <kbd>]</kbd> — Show a list of matching definitions if there are duplicates.

~~~~
cd /path/to/your/project
ctags -f tags -R --fields=+K+a
~~~~
