---
layout: default
---


execute ctags to generate tags for you project. After this step Ctrl+] will go to definition.

~~~~
cd /path/to/your/project
ctags -f tags -R --fields=+K+a
~~~~
