---
layout: default
---

# A Quick Guide About Using Vim 

<p class="topshort">Below is a organized list of keys and commands for vim.</p>

![sea, underwater fish](/images/fish.png){:style="width: 80%; display: block;float: none;margin-left: auto;margin-right: auto;margin-top: 50px;margin-bottom: 50px;"} 


## Using Vim

When you open a file set the current directory 
~~~~~~~~~~~~~~~ cmd
:cd %:p:h
~~~~~~~~~~~~~~~

Modes
----------------

| Key | Mode |
| :--- | :--- |
| <kbd>i</kbd> |   Insert mode |
| <kbd>R</kbd> |   Replace mode |
| <kbd>v</kbd> |   Visual mode |
| <kbd>V</kbd> |   Visual line mode |
| <kbd>Ctrl</kbd> + <kbd>V</kbd> |   Visual block mode |
| <kbd>:</kbd> |   Command line mode |
| <kbd>Esc</kbd> |   Return to normal mode |

Editing
----------------

| Key | Insertion Behavior |
| :----- | :----- |
| <kbd>a</kbd> | Insert after cursor |
| <kbd>A</kbd> | Insert after line |
| [n]<kbd>i</kbd> | Insert text before the cursor n times |
| [n]<kbd>I</kbd> | Insert text before the first char of the line n times |
| [n]<kbd>a</kbd> | Append text after the cursor n times |
| [n]<kbd>A</kbd> | Append text after the end of line n times |
| [n]<kbd>o</kbd> | Begin a new line below the cursor and insert text, repeat n times |
| [n]<kbd>O</kbd> | Begin a new line above the cursor and insert text, repeat n times |
| [n]["x]<kbd>c</kbd><kbd>c</kbd> | Delete current line [into register x] and enter insert mode |
| [n]["x]<kbd>C</kbd> | same as c$ but also take into register |
| <kbd>c</kbd><kbd>$</kbd> | Delete from the cursor position to the end of the line and enter insert mode |
| <kbd>c</kbd><kbd>w</kbd> | Delete from the cursor position to the end of the current word and enter insert mode |
| <kbd>c</kbd><kbd>e</kbd> | Delete from the cursor position to the end of the current word and enter insert mode |
| <kbd>c</kbd><kbd>i</kbd><kbd>w</kbd> | Delete the current inner word and enter insert mode |
| [n]<kbd>y</kbd><kbd>y</kbd> | Yank the current n lines, default is only the current line |
| <kbd>y</kbd><kbd>i</kbd><kbd>w</kbd> | Yank inner word |
| <kbd>y</kbd><kbd>$</kbd> | Yank to the end of current line |
| [n]["x]<kbd>s</kbd> | (substitute) delete n characters [into register x] and start insert | 
| [n]["x]<kbd>S</kbd> | (substitute) delete n lines [into register x] and start insert |

Movement
----------------

| Key | Movement Behavior |
| :--- | :--- |
| <kbd>H</kbd> | Left   |
| <kbd>J</kbd> | Down |
| <kbd>K</kbd> | Up |
| <kbd>L</kbd> | Right |
| [n]<kbd>w</kbd> | Jump to start of nth word |
| [n]<kbd>e</kbd> | Jump to end of nth word |
| [n]<kbd>b</kbd> | Jump to start previous nth word |
| [n]<kbd>h</kbd> | Cursor n chars to the left |
| [n]<kbd>l</kbd> | Cursor n chars to the right |
| [n]<kbd>j</kbd> | Cursor n lines downward |
| [n]<kbd>k</kbd> | Cursor n lines upward |
| [n]<kbd>H</kbd> | Cursor to the line n from top of screen |
| <kbd>M</kbd> | Cursor to the middle of screen |
| [n]<kbd>L</kbd> | Cursor to the line n from bottom of screen |
| [n]<kbd>g</kbd><kbd>g</kbd> | Cursor to line n, default first line |
| [n]<kbd>G</kbd> | Cursor to line n, default last line |
| <kbd>0</kbd> | Jump to beginning of line |
| <kbd>$</kbd> | Jump to end of line |
| <kbd>^</kbd> | Jump to beginning character of line |
| <kbd>g</kbd> + <kbd>g</kbd> | Go to the end of document |
| <kbd>G</kbd> | Go to the beginning of document |
| <kbd>Ctrl</kbd><kbd>D</kbd> | Move cursor down half page |
| <kbd>Ctrl</kbd><kbd>U</kbd> | Move cursor up half page |
| <kbd>Ctrl</kbd><kbd>B</kbd> | Move screen up one page |
| <kbd>Ctrl</kbd><kbd>F</kbd> | Move screen down one page |

Search
----------------

| Key | Search Behavior |
| :--- | :--- |
| <kbd>f</kbd> + <kbd>s</kbd> | Find first occurrence of 's' after the cursor |
| <kbd>;</kbd> | Repeat last find forward |
| <kbd>,</kbd> | Repeat last find backwards |


Windowing
----------------

| Key | Window Behavior |
| :--- | :--- |
| [n]<kbd>Ctrl</kbd><kbd>W</kbd> <kbd>s</kbd> | Split current window in two parts, new window n lines high |
| [n]<kbd>Ctrl</kbd><kbd>W</kbd> <kbd>v</kbd> | Split current window vertically, new window n columns wide |

to open a file in a split window
~~~~ vim
:split /file_path
:vsplit /file_path
~~~~

<kbd>Ctrl</kbd><kbd>W</kbd> <kbd>c</kbd> Close current split window

[n]<kbd>Ctrl</kbd><kbd>W</kbd> <kbd>h</kbd> Go to nth left window (stop at first window)
[n]<kbd>Ctrl</kbd><kbd>W</kbd> <kbd>j</kbd> Go to n windows down (stop at last window)
[n]<kbd>Ctrl</kbd><kbd>W</kbd> <kbd>k</kbd> Go to n windows up (stop at first window)
[n]<kbd>Ctrl</kbd><kbd>W</kbd> <kbd>l</kbd> Go to nth right window (stop at last window)
[n]<kbd>Ctrl</kbd><kbd>W</kbd> <kbd>x</kbd> Exchange current window with window n (default: next window)

To keep the window number visible across all split viewports, add it to your status line format string. Open your configuration file (usually ~/.vimrc) and append the following syntax:

~~~~ vim
set statusline+=Window:\ %{winnr()}
~~~~

~~~~ vim
" Always display the statusline
set laststatus=2

" Clear any existing statusline configuration
set statusline=

" Left Side Info
set statusline+=%f\                     " File path
set statusline+=%m                      " Modified flag
set statusline+=%r                      " Read-only flag
set statusline+=%h                      " Help file flag

" Right Side Info (pushed via %=)
set statusline+=%=                      " Switch to right side alignment
set statusline+=[%Y]\                   " File type
set statusline+=[%l/%L]\                " Line number / Total lines
set statusline+=col:%c\                 " Column number
set statusline+=(%p%%)                  " Percentage through file
~~~~


To open an existing buffer in a new window in Vim, run 

:sbuffer [N] (for a horizontal split) or 

:vert sbuffer [N] (for a vertical split), replacing [N] with the buffer's number or name

Split to the Right: 

:vert belowright sbuffer [N]


To toggle line numbers:

~~~~ vim
set number!
~~~~


~~~~ vim
" Ensure the status line is always visible
set laststatus=2

" Set color for the active window status line
highlight StatusLine ctermfg=white ctermbg=darkblue guifg=#ffffff guibg=#00008b

" Set color for inactive window status lines
highlight StatusLineNC ctermfg=gray ctermbg=darkgray guifg=#a0a0a0 guibg=#444444
~~~~

<kbd>"</kbd><kbd>+</kbd><kbd>y</kbd>                Copy word into system clipboard
<kbd>"</kbd><kbd>+</kbd><kbd>y</kbd><kbd>y</kbd>    Copy line into system clipboard

<kbd>"</kbd><kbd>+</kbd><kbd>p</kbd>     Paste from system clipboard

<kbd>y</kbd> + <kbd>w</kbd>   Copy word
<kbd>d</kbd> + <kbd>w</kbd>   Delete word (cut)

<kbd>y</kbd> + <kbd>y</kbd>   Copy line
<kbd>d</kbd> + <kbd>d</kbd>   Delete line (cut)
 
<kbd>p</kbd>   Paste after the cursor/line

<kbd>P</kbd>   Paste before the cursor/line

<kbd>u</kbd>   Undo

<kbd>v</kbd>   Visual mode

<kbd>V</kbd>   Line visual mode

[n]<kbd>r</kbd>{char}   Replace n chars with {char}
<kbd>R</kbd>   Replace mode

[n]<kbd>r</kbd>{char}   Replace n chars with {char}

<kbd>J</kbd>              Join lines default is 2
<kbd>g</kbd><kbd>J</kbd>  Join lines without inserting space

<kbd>%</kbd>   Go to matching scope beginning/end (paranthessis, curly braces, etc. )

<kbd>d</kbd> + <kbd>i</kbd> + <kbd>w</kbd>   Delete in word

<kbd>d</kbd> + <kbd>i</kbd> + <kbd>(</kbd>   Delete in paranthesis

[n]<kbd>d</kbd> + <kbd>$</kbd> Delete from the cursor to the end of the line (and the next n - 1 lines)
[n]<kbd>D</kbd>   Delete from the cursor to the end of the line (and the next n - 1 lines)
[n]["x]<kbd>x</kbd> Delete n characters under and after the cursor [into register x]

<kbd>c</kbd> + <kbd>i</kbd> + <kbd>(</kbd>   Change in paranthesis

<kbd>Ctrl</kbd> + <kbd>o</kbd>   Cursor to previous position

<kbd>Ctrl</kbd> + <kbd>i</kbd>   Cursor to next position

Search and replace
-----------------------

* Execute a regular search with <kbd>/</kbd>
* Use the keystroke <kbd>c</kbd> + <kbd>g</kbd> + <kbd>n</kbd> on the first result to replace it.
* Position cursor on a word and press <kbd>*</kbd> to search forward
* Position cursor on a word and press <kbd>#</kbd> to search backward
* Type <kbd>n</kbd> or <kbd>N</kbd> to go to the next result.
* Use <kbd>.</kbd> to replace the occurrence with the same replacement, or go to the next result if you want to keep it.
* Search /word
* Search exact /\<word\>
* Search case insensitive /word\c

To replace all occurrences of a term in the entire file in Vim, press Esc to enter Normal mode and run 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ vim
:%s/old_text/new_text/g
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

(% Specifies the range as the entire file. s Calls the substitute command. g Stands for global, replacing every match on a line rather than just the first one.)

Confirm each change -> Add the c flag to accept or reject every instance interactively:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ vim
:%s/old_text/new_text/gc   (Press y to replace, n to skip, or a to replace all remaining)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ignore case sensitivity -> Add the i flag if you want to find matches regardless of capitalization:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ vim
:%s/old_text/new_text/gi
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Replace exact whole words only -> Wrap your text in word boundary markers (\< and \>) so it doesn't modify partial matches inside longer words:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ vim
:%s/\<old_text\>/new_text/g
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use alternative delimiters if your text contains slash characters (like file paths or URLs), use a different delimiter like # or | to avoid escaping them:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ vim
:%s#/var/www/old#/var/www/new#g
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Entire File      :%s/foo/bar/g      Replaces all instances of foo with bar.
Current Line     :s/foo/bar/g       Replaces all instances on your active line only.
Line Range       :5,12s/foo/bar/g   Replaces all instances between lines 5 and 12.


Find and Replace in Multiple Files
------------------------------------

If you want to replace an occurrence in every html and twig files, you can type the following:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ vim
:arg *.html - Populate the arglist with all html files in the current working directory, and edit the first one.
:argadd *.twig - Add twig files to the arglist.
:argdo %s/pattern/replace/ge | update - Replace the occurence pattern by replace in every file of the arglist.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Even if the argument list (:help arglist) and the buffer list (:help :buffers) are different, every files added in the argument list will be added in the buffer list.

You can delete files in one of these lists without changing the other. For example, :argdelete * will remove everything in your argument list.

At that point, you might scream to your screen, violently shacked by extrem curiosity: what means the flag e at the end of the substitute command? It prevents Vim to display an error message when the pattern is not found in a file.

What about doing a find and replace in the working directory and the subdirectories? You can populate the arglist as follow:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ vim
:arg **/*.html
:argadd **/*.php
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

How to Indent
---------------

<kbd>></kbd> + <kbd>i</kbd> + <kbd>}</kbd>   increase indent

<kbd><</kbd> + <kbd>i</kbd> + <kbd>}</kbd>   decrease indent

<kbd>=</kbd> + <kbd>i</kbd> + <kbd>B</kbd>   autoindent block

<kbd>></kbd> + <kbd>%</kbd>     put your cursor on one of the curly braces

If you’re copying blocks of text around and need to align the indent of a block in its new location, use <kbd>]</kbd>+<kbd>p</kbd> instead of just p. This aligns the pasted block with the surrounding text.

Source init.lua
---------------

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ vim
:luafile init.lua
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


# using ctags with vim

execute ctags to generate tags for you project. 

After this step <kbd>Ctrl</kbd> + <kbd>]</kbd> will go to definition.

<kbd>g</kbd> then <kbd>Ctrl</kbd> + <kbd>]</kbd> — Show a list of matching definitions if there are duplicates.

~~~~ shell
cd /path/to/your/project
ctags -f tags -R --fields=+K+a
~~~~

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ cmd
ctags -R .
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


In your ~/.vimrc file, add this short block:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ vim
 " ctags optimization
 set autochdir
 set tags=tags;
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

" denotes a comment. set autochdir tells vim that if it doesn't find a tags file in the $PWD it will look in the directory parent for the tags file, recursively. set tags=tags; tells vim that the name of your tags file will always be the same as the default tags file generated by ctags.


<kbd>Ctrl</kbd>+<kbd>]</kbd> - go to definition

<kbd>Ctrl</kbd>+<kbd>T</kbd> - Jump back from the definition.

<kbd>Ctrl</kbd>+<kbd>W</kbd> <kbd>Ctrl</kbd>+<kbd>]</kbd> - Open the definition in a horizontal split

<kbd>Ctrl</kbd> + <kbd>t</kbd>: Jump back up one level in your tag stack.

<kbd>g</kbd> + <kbd>Ctrl</kbd> + <kbd>]</kbd>: View a list of matching definitions if the name is used in multiple places.

<kbd>Ctrl</kbd> + <kbd>x</kbd> then <kbd>Ctrl</kbd> + <kbd>]</kbd>: Trigger auto-completion in Insert Mode using your tags file.


Add these lines in vimrc

map <C-\> :tab split<CR>:exec("tag ".expand("<cword>"))<CR>

map <A-]> :vsp <CR>:exec("tag ".expand("<cword>"))<CR>

<kbd>Ctrl</kbd>+<kbd>\</kbd> - Open the definition in a new tab

<kbd>Alt</kbd>+<kbd>]</kbd> - Open the definition in a vertical split

After the tags are generated. You can use the following keys to tag into and tag out of functions:

<kbd>Ctrl</kbd>+Left MouseClick - Go to definition

<kbd>Ctrl</kbd>+Right MouseClick - Jump back from definition


Terminal Commands
----------------------

To run a terminal command without input or output
~~~~~~ vim
:!{command}
~~~~~~

To run a terminal command and pass the output to current buffer
~~~~~~ vim
:read !{command}
~~~~~~

Macros
-------------------

| Key | Macro Behavior |
| :---: | :--- |
| q{0-9a-zA-Z"} | Record typed characters into named register {0-9a-zA-Z"}(uppercase to append) |
| q | (while recording) stops recording |
| [n]@{a-z} | Execute the contents of register {a-z} n times |
| [n]@@ | Repeat the previously executed macro n times |


# Sample vimrc


~~~~~~ vim
syntax on
set background=dark
colorscheme desert

" Always display the statusline
set laststatus=2

" Clear any existing statusline configuration
set statusline=

" Left Side Info
set statusline+=%F\                     " Full path to the file
set statusline+=%m                      " Modified flag

" Right Side Info (pushed via %=)
set statusline+=%=                      " Switch to right side alignment
set statusline+=Window:\ %{winnr()}
set statusline+=\ %y                      " File type
set statusline+=\ [%L\ lines]                " Line number / Total lines
~~~~~~
