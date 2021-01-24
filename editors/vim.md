# Vim

## Contents
- Grammar
  - Key terms
  - Structure of a command

## Grammar

### Key Terms
- *Operator*
  - The verbs: stuff you do to text.
  - Main Examples: d, y, c.
- *Motion*
  - Any key or keys that moves your cursor within the document.
  - Examples: $, gg, e, b
- *Text object*
  - Consists of a modifier + object. Both are required.
  - *Modifier*: either a or i
    - a: "around", ie: includes surrounding whitespace and/or symbols
    - i: "inside", ie: excludes surrounding whitespace and/or symbols
  - *Object*
    - Examples:
      - s: sentence 
      - p: paragraph
      - [ ] { } ( ) " ' ` < >: all specify objects wrapped by their corresponding character`
  - Examples
    - `das`: delete current sentence including surrounding whitespace.
    - `vi'`: visually select inside a string, without including the `'` character.
- *Repetition*.
  - Any operator, motion or text-object can be preceded with a number to say "do this X times".
  - eg `3e` repeats the `e` motion 3 times.

### Structure

- operator ( motion | text-object )
  - Operators, motions and text-objects all take an optional repetition
    - ie: `[repetion] operator ( [repetion] motion | [repetition] text-object )`

  - Examples:
    - `3yw`
    - `2das`: delete around current sentence and the next.

## Interesting keys I didn't know about
Commands
- gU: uppercase
- gu: lowercase

Motions:
- H: move to top of screen
- M: move to middle of screen
- L: move to bottom of screen
  
Screen:
- zz: center window on current line
- zt: top this line
- zb: bottom this line

## Ex Commands
- `:read somefile.rb`: yank the contents of somefile and paste after cursor.

## Useful Plugin Commands

*Vim Surround*
- uys`+text-object+punctuation: surround a text object with punctuation.
  - eg: `ysiW'`: surround current word with `'`.
- Visually select then `S`+punctuation
  

## Sources
- [Cheatsheet](https://devhints.io/vim#operators)
- [Vim grammar](https://takac.github.io/2013/01/30/vim-grammar/)
