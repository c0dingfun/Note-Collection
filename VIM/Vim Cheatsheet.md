[My Vim Cheatsheet](http://vimcasts.org/episodes/ "reference")
====

Command Pattern
----

```vim
["register][repeats][operator][motion/text-object]
```

|item |meaning |
|:--| :-- |
|["register]|register name (optional) eg: "b |
|[repeats]|repeats (options), eg: 13 |
|[operator]|operator, eg: y (yank)|
|[motion/text-object]|motion or text-object, eg: d} - delete to end of a function|

Operators
----

|operator| meaning|
|:--|:--|
|["x]c{motion}| change|
|["x]d{motion}| delete|
|["x]y{motion}| yank into register (no text change)|
|||
|~{motion}| swap case, character-wise |
|g~{motion}|swap case. default current line |
|g~~ | swap case the current line|
|||
|gu{motion} / gU{motion}|make lowercase / uppercase, default current line|
|guu / gugu | make lowercase, current line|
|gUU / gUgU | make lowercase, current line|
|||
|g?{motion}| rot13 encode, default current line|
|g?? / g?g?| rot13 encode current line|
|||
|!| filter through an external program|
|={motion}| indenting|
|gq{motion}|text formatting|
|>{motion}|shift right|
|<{motion}|shift left|
|zf{motion} / {visual}zf|define a fold (see Folding)|

Motions
----

- What is a word (aka normal word) and what is a WORD (aka big WORD)?
- [A very good explanation](:https://stackoverflow.com/questions/22931032/vim-word-vs-word)

  - A word ends at a non-word character, such as a "white-space", ".", "-", "," or ")".
  - A WORD ends strictly with a white-space.

```vim
         ge      b          w                          e
         <-     <-         --->                       --->
  This is-a line, with special/sep"ed/words (and some more). ~
     <----- <-----         ----------------->         ----->^
       gE      B                    W                   E   |--space
  <---------------             ------------>      <---- ----->
         ^                         t(               Ts    $
<-----------------             ------------->    <-----
         0                         f(               F
```

If cursor is at 'm' (of "more" above)

- a word would mean 'more' (i.e delimited by ')' non-word character ")"

- whereas a WORD would mean 'more).' (i.e. delimited by white-space only)

similarly, If your cursor is at p (of special)

- a word would mean 'special'
- whereas a WORD would mean 'special/separated/words'

|motion| meaning|
|--|--|
|word motions|-----|
|w / W | forward to begin of Normal / Big Word|
|b / B | backward to begin of Normal / Big Word|
|e / E | forward to end of Normal / Big Word|
|ge / gE | backward to end of Normal / Big Word|
||-----|
|left-right motions|-----|
|h / l  | character left / right|
|j / k | line down / up|
|+ / j^| go to beginning of next line|
|- / k^| go to beginning of previous line|
|{n}`|`|motion to "n" column |
||-----|
|0 | beginning of the line|
|g0 | first character of **screen** line|
||-----|
|^ | first non-white space on line|
|g^| first **screen** character on line|
||-----|
|_ | first non-white space on line (with count)|
|g_ | last non white space on the line|
||-----|
|$ | end of line|
|g$ | last **screen** character on line|
||-----|
|||
|t{char} / T{char} | move forward / backward to before character, eg: tx / eg: Tx|
|f{char} / F{char} | move forward / backward onto character, eg: fx / eg: Fx|
|; | repeat t, T, f, F|
|, | repeat t, T, f, F|
|||
|up-down motions|-----|
|gg | go to top|
|G  | go to bottom|
|nG | go to line n|
|:n | go to line n|
|||
|search commands|-----|
|* / # | (whole word) search forward / backward the `<n>` occurrence of the word nearest to cursor|
|g* / g# |(include partial) search forward / backward the `<n>` occurrence of the word nearest to cursor|
|n / N | go to next match forward / backward|
|gn / gN | go to next match, forward / backward, and in **visual** mode |
|-->| if an operator is pending, operates on the match, eg: dgn deletes the text of the next pattern|
|||
|position cursor|-----|
|H |cursor to top of the screen|
|M |cursor to middle of the screen|
|L |cursor to bottom of the screen|
|||
|reposition current line|-----|
|zt| move current line to top
|zz| move current line to middle
|zb| move current line to bottom
|||
|ctrl-u / ctrl-d|scroll up/down|
|||
|% | go to matching ([{}])|
|||
|text-object motions|-----|
|( | Sentence backward|
|) | Sentence forward|
|{ | Paragraph backward|
|} | Paragraph forward|
|]]||
|[[||
|[]||
|][||

Visual Mode
----

|operator | meaning |
|---|---|
|v | character-wise **visual** mode|
|V | line-wise **visual** mode|
|ctrl-v or ctrl-q| block-wise **visual** mode|
|gv| re-select the last visual|

[Text-Object](https://blog.carbonfive.com/2011/10/17/vim-text-objects-the-definitive-guide/)
----

|item| meaning|
|--|--|
|text object | -----|
|aw | a(n) word, including surrounding white space|
|iw | i(nner) word, not including surrounding white space|
|as | a(n) sentence|
|is | i(nner) sentence|
|ap | a(n) paragraph|
|ip | i(nner) paragraph|
|   ||
|programming text object | -----|
|a" | a double quoted string|
|i" | inner double quoted string|
|a' | single quoted string|
|i' | inner single quoted string|
|   ||
|a) | a parenthesized block|
|i) | inner parathesizd block|
|ab | a parenthesized block|
|ib | inner parathesizd block|
|   ||
|a] | a bracketed block|
|i] | inner bracketed block|
|a} | a brace block|
|i} | inner brace block|
|aB | |
|iB ||
|at | a tag block|
|it | inner tag block|

Tabbing
----

|command|meaning|
|--|--|
| gt / gT / `<n>`gt | switching tab forward / backward / number tab, eg: 3gt|
|:tabe[dit] [file] | edit file in a new tab|
|:tabf[ind] [file] | open file if exists in a new tab|
|:tabc[lose] | close the current tab|
|:tabs| list all tabs|
|:tabfir[st]| go to first tab|
|:tabl[ast]| go to last tab|
|:tabn| go to next tab|
|:tabp| go to previous tab|

Folding
----

- Fold Setup

```vim
fdc (fold column) :set fdc=4
fdm (fold method) :set fdm=manual, indent
fen (fold enable) :set nofen   :set fen
fdl (fold level)  :set fdl=1
```

|fold op| meaning|
|--|--|
|zf{visual}/{motion} |fold {visual}/{motion}, eg: zf4j (fold next 4 lines)|
|{count}zF| fold [count] lines|
||-----|
|zo / zO |open fold / recursively|
|zc / zC |close fold / recursively|
|za / zA | toggle fold/unfold|
||-----|
|zv| open folds for this line|
||-----|
|zM| close all folds|
|zR| open all folds|
||-----|
|zi| toggle fold enable/disable|
||-----|
|zj / zk| move cursor to next / previous fold|
||-----|
|zd / zD| delete fold at the cursor / recursively|
|zE| delete all folds|
|zm| increase fold|
|zr| reduce fold|

Navigation
----

|operator|meaning|
|---|---|
|[{ |previous {|
|]} |next } |
|[m / [M | previous method start / end|
|ctrl-o / ctrol-i| up / down the list of recent positions|

Spelling
----

|what to do| meaning|
|---|---|
|:set spell| enable spelling|
|]s / [s | move to next /previous misspelled word after cursor
|z=|list suggested spellings for the word under/after the cursor|
|zg|add word to spell list|

Starting Vim
----

```vim
vim -O file1, file2 // Vertical
vim -o file1, file2 // horizontal
vim -p file1, file2 // individual tabs

// Ctrl-W (H, L) switching left/right
// Ctrl-W (K, J) switching top/down

// gt or gT or `<n>`gt  switching tabs

:e!            // reload the file, without saving changes
:split file    // splits window horizontally
:vsplit file   // splits window vertically
```


Quitting Vim
----

```vim
:qa   // quit all - if not changes
:qa!  // quit all - no saving

ZZ    // save and quit
ZQ    // quit without saving

:wq   // save and quit
:x    // save and quit

:cq 
:cquit // quitting with error
```

Undo and Redo
----

```vim
u        // undo
U        // undo all changes for the line
ctrl-R   // redo
```

Vim Help
----

```vim
Ctrl-]   // following the link and put current place on the stack
Ctrl-T   // go to the place pop from the stack
```

Enter Insert Mode
----

```vim
i  // insert before the current cursor
I  // insert at the beginning of the current line
a  // insert after the current cursor
A  // insert at the end of the current line
r  // retype just the character under the cursor
R  // ENTER overtype (replace) mode, until ESC is enter
s  // substitute character under the cursor

c{movement} // change (retype) to {motion} on current line - like cw
C = cc      // change (retype) current line (the whole line)
<n>cc       // change (retype) on <n> lines

cc          // change (retype) current line
dd          // delete current line
D           // delete to the end of the line
yy          // copy (yank) the current line
gg          // go to the top of the file
ZZ          // save and quit the current file
```

[Registers](https://www.brianstorti.com/vim-registers/)
----

- Get Help, :help registers

|Register Name|What It Does|
|---|---|
|"|the unnamed or default or clipboard register, (for cut/paste) eg: ""p|
|0-9| rolling registers, 0 for yank, 1-9 for deletes|
|-| small delete register|
|a-z| local registers for current file |
|A-Z| global registers|
|:| (read only), stores the last command, eg: @: will execute the last command again|
|.| (read only), stores last inserted text)|
|%| (read only), stores the current file path|
|#| (read only), stores the alternative file|
|=|expression register|
|*|select/drop register|
|+|select/drop register|
|~|select/drop register|
|_| black hole register |
|/| search register|

```vim
"add  // delete the current line into register a
:reg a   // show  register a content
:reg     // show all registers' content
ctrl-r a // in insert-mode, paste register a
ctrl-a   // last inserted text
```

```vim
"ayy  // cut the current line into register a
"ap   // paste from register a
22"ap // paste from register a, 23 times
```

Completion (not working in VS/Code :-) )
----

```dotnetcli
ctrl-n   // in insert-mode, complete a word, cycling from top to bottom
ctrl-p   // in insert-mode, complete a word, cycling from bottom to top
ctrl-x ctrl-l  // in insert-mode, complete a sentence
ctrl-x ctrl-f  // file name completion

ctrl+p   // find the previous matching completion for the partially typed word
ctrl+n   // find the previous matching completion for the partially typed word
```

Status Line Hints (not working in VS/Cod)
----

```vim
[i    // show first line containing work under the cursor
[I    // show all line containing work under the cursor
```

File Info
----

```vim
ctrl-g   // prints current file name, cursor position, and file status
```

Encryption
-----

- use :setlocal cm=blowfish2

[IDE](http://vim.wikia.com/wiki/Use_Vim_like_an_IDE)
----

[CTags](https://benoithamelin.tumblr.com/post/15101202004/using-vim-exuberant-ctags-easy-source-navigation)
----

Moving around in Vim Help
----

```iecst
   > Tag Jumping (to specific subjects)
      CTRL-]
      double click
      Ctrl-LeftMouse
      g-LeftMouse

   > Jump back
      CTRL-T 
      CTRL-O 

   > :Help <topic> Ctrl-D  // to list all related helps

```

- gf - go to the file under cursor
- `<c-w><c-f>` open the file under cursor in a split window
- gd - go to definition

Marker
---

- '' - back and forth, location
- ma - mark a
- 'a - go to mark a

Some VimDiff keyboard shortcuts
----

- do - Get changes from other window into the current window. [not working for me on Win7]
- dp - Put the changes from current window into the other window.

- ]c - Jump to the next change.
- [c - Jump to the previous change.

- Ctrl W + Ctrl W - Switch to the other split window.

- If you load up two files in splits (:vs or :sp), you can do 

```vim
:diffthis on each window and achieve a diff of files that were already loaded in buffers
:diffoff can be used to turn off the diff mode.
```

Vim Tips
----

   Vim + Tmux
   * https://www.youtube.com/watch?v=5r6yzFEXajQ
   * https://www.youtube.com/watch?v=aHm36-na4-4
   * https://www.youtube.com/watch?v=lZdkUK2jgGY
   * https://www.youtube.com/watch?v=YD9aFIvlQYs

[Where is VsVim's .vsvimrc file](https://codeyarns.com/2013/12/05/how-to-use-vimrc-with-vsvim/)
----

```dotnetcli
   :set vimrcpaths?
   :set vimrc?
```

[Remove Duplicate Lines](http://stackoverflow.com/questions/2738826/removing-contiguous-duplicate-lines-in-vi-without-sorting)
----

- [Cheatsheet](https://devhints.io/vim)

Fun Stuff
----

- in text mode
```vim
   <C-R> = 128 * 22
```

- :set rightleft
- :help uganda
- :help!
- :help 42
- :help quotes
- :help holy-grail

MISC
----

```vim
ctrl-q   // alternative to ctrl-v
[p / [P  // like p/P, but adjusted to the indent
]p / ]P  // like p/P, but adjusted to the indent
&        // repeat last substitution
ga       // print the ascii value of the character under the cursor (on status line)
g8       // print hex values of bytes used in the character under cursor
gJ       // join lines
```

[References]
----

[https://www.cs.swarthmore.edu/oldhelp/vim/][1]
[https://dalibornasevic.com/posts/43-12-vim-tips][2]
[https://vim.fandom.com/wiki/Best_Vim_Tips][3]
[https://learnvimscriptthehardway.stevelosh.com/chapters/10.html][4]
[https://learnvimscriptthehardway.stevelosh.com/][5]
[https://bennetthardwick.com/blog/2019-01-06-beginner-advanced-vim-tips-and-tricks/][6]
[https://www.hillelwayne.com/post/intermediate-vim/][7]
[https://www.rosehosting.com/blog/vim-tips-and-tricks/][8]
[http://zzapper.co.uk/vimtips.html][9]
[https://www.ukuug.org/events/linux2004/programme/paper-SMyers/Linux_2004_slides/vim_tips/][10]
[http://vimcasts.org/episodes/][11]
[https://georgebrock.github.io/talks/vim-completion/][12]
[https://www.hillelwayne.com/post/intermediate-vim/][13]
[https://bennetthardwick.com/blog/2019-01-06-beginner-advanced-vim-tips-and-tricks/][14]
[https://itsfoss.com/pro-vim-tips/][15]
[https://dalibornasevic.com/posts/43-12-vim-tips][16]