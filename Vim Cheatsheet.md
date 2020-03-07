# Vim Cheatsheet

## Encryption

- to use best encryption, do:

- use :setlocal cm=blowfish2

## [IDE](http://vim.wikia.com/wiki/Use_Vim_like_an_IDE)

## Online Help

```iecst

   > Tag Jumping (to specific subjects)
      CTRL-]
      <C-LeftMouse>
      g<LeftMouse>

   > Jump back
      CTRL-T  (tag stack)
      CTRL-O  (jumplist)
```

## Fold

```vim

   * fdc (fold column) :set fdc=4
   * fdm (fold method) :set fdm=manual, indent
   * fen (fold enable) :set nofen   :set fen
   *
   * zf (do a fold—a highlighted region)
   * zc (close fold)
   * zo (open fold)
   * za (toggle fold/unfold)
   * zi (invert fold enable/disable)
   * zm (increase fold )
   * zr (reduce fold)
```

* gf - go to the file under cursor
* <c-w><c-f> open the file under cursor in a split window
* gd - go to definition

## marker

   - '' - back and forth, location
   - ma - mark a
   - 'a - go to mark a

## Remove Duplicate Lines
http://stackoverflow.com/questions/2738826/removing-contiguous-duplicate-lines-in-vi-without-sorting

If you want to remove non-contiguous duplicates you could use
:g/^\(.*\)\ze\n\%(.*\n\)*\1$/d

(which will remove all but the last copy of a line)

which would change 
Foo
Bar
Foo
Bar
Foo
Baz
Foo
Quux

to 
Bar
Baz
Foo
Quux

If you want to remove all but the first copy, try
:g/^/m0
:g/^\(.*\)\ze\n\%(.*\n\)*\1$/d
:g/^/m0

which would change
Foo
Bar
Foo
Bar
Foo
Baz
Foo
Quux

to
Foo
Bar
Baz
Quux

## Some Vimdiff keyboard shortcuts

- do - Get changes from other window into the current window. [not working for me on Win7]
- dp - Put the changes from current window into the other window.

- ]c - Jump to the next change.
- [c - Jump to the previous change.

- Ctrl W + Ctrl W - Switch to the other split window.

If you load up two files in splits (:vs or :sp), you can do 
:diffthis on each window and achieve a diff of files that were already loaded in buffers
:diffoff can be used to turn off the diff mode. 

# Text Objects

https://blog.carbonfive.com/2011/10/17/vim-text-objects-the-definitive-guide/

Plaintext Text Objects
Vim provides text objects for the three building blocks of plaintext: words, sentences and paragraphs.

   = Words
      aw – a word (includes surrounding white space)
      iw – inner word (does not include surrounding white space)

   = Sentences
      as – a sentence
      is – inner sentence

   = Paragraphs
      ap – a paragraph
      ip – inner paragraph

## Programming Language Text Objects

   - Strings

```iecst

      a" – a double quoted string
      i" – inner double quoted string
      a' – a single quoted string
      i' – inner single quoted string
      a` – a back quoted string
      i` – inner back quoted string

```
   - Parentheses


```iecst


      a) – a parenthesized block
      i) – inner parenthesized block
```

   - Brackets

```iecst
      a] – a bracketed block
      i] – inner bracketed block
```
   - Braces
```iecst
      a} – a brace block
      i} – inner brace block
```
   - Markup Language Tags
```iecst

      at – a tag block
      it – inner tag block

      a> – a single tag
      i> – inner single tag

```

## Custom Text Objects

   -  VimTextObj
```iecst

      aa – an argument
      ia – inner argument
```

   - Indent Object

```iecst

      ai – the current indentation level and the line above
      ii – the current indentation level excluding the line above

```