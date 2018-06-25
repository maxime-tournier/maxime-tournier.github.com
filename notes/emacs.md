---
title: Emacs
---

Quick cheat sheet.

# Macros `C-x C-k`

- `F3` start recording a macro
- `F4` stop recording or replay last recorded macro

- `C-x C-k b <i>` bind macro to key `<i>`
- `C-x C-k <i>` replay macro with key `<i>`

[more](https://www.gnu.org/software/emacs/manual/html_node/emacs/Save-Keyboard-Macro.html)


# Mark

- `C-SPC` set the mark
- `C-x C-SPC` pop global mark
- `C-u C-SPC` pop local mark
- `C-x C-x` switch point/mark + select

# Registers `C-x r`

- `C-x r s <i>` save text to register `<i>`
- `C-x r i <i>` insert text from register `<i>`

[more](https://www.gnu.org/software/emacs/manual/html_node/emacs/Text-Registers.html#Text-Registers)


# Narrowing `C-x n`

- `C-x n d` narrow to function
- `C-x n w` widen to buffer


# Diffing

- `M-x smerge-ediff` resolve conflicts using ediff

