# Emacs

I currently use Doom as my text editor. It's an Emacs distribution which uses Evil mode - that is, Vim keybindings. My goal for these notes is therefore to learn just enough Emacs that I can use Doom effectively.

## Key Concepts
- Keys & Commands
- Modes

## Keys & Commands
  - A command is a named lisp function that performs an action.
  - A command may or may not have a key binding.
  - The bindings between keys and commands are stored in tables called keymaps.
  - Use `M-x` to invoke commands by name.
  - Examples of commands: `help-command`, `find-file`, `save-buffer`.

## Modes

### Major modes
- Every buffer has one major mode.
- It usually corresponds to the name of the language that you're coding in - for example a `.rb` file will use the `ruby-mode`.
- The major mode provides features like syntax highlighting, automatic indentation, etc.

### Minor modes
- Multiple minor modes can exist in a single buffer.
- Minor modes can be buffer-specific or global.
- Minor modes can be enabled or disabled using their corresponding command, which is always `<mode name>-mode`.
  - Access these commands using `M-x`.
  - eg run `auto-fill-mode` to toggle Auto Fill minor mode.
- Examples of minor modes: Auto Save, Fly Spell (underlines misspelled words), Display Line Numbers.

## Useful commands
- describe-key (SPC h k): Display docs for a given keybinding 

## Sources
- Emacs docs: https://www.gnu.org/software/emacs/manual/html_node/emacs/index.html
