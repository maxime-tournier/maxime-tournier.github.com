---
title: Emacs
---

Quick cheat sheet.

# Macros `C-x C-k`

- `F3` start recording a macro
- `F4` stop recording or replay last recorded macro

- `C-x C-k b <i>` bind macro to key `<i>`
- `C-x C-k <i>` replay macro with key `<i>`
- `C-x C-k n` name last macro (use with `M-x insert-kbd-macro` to save macros)
- `C-x C-k C-e` edit last macro (when you screwed up and don't want to start over)

[more](https://www.gnu.org/software/emacs/manual/html_node/emacs/Save-Keyboard-Macro.html)


# Mark

- `C-SPC` set the mark
- `C-x C-SPC` pop global mark
- `C-u C-SPC` pop local mark
- `C-x C-x` switch point/mark + select

# Registers `C-x r`

- `C-x r s <i>` save text to register `<i>`
- `C-x r i <i>` insert text from register `<i>`

## Position Registers

- `C-x r SPC <i>` save point to register `<i>`
- `C-x r j <i>` jump to register `<i>`

[more](https://www.gnu.org/software/emacs/manual/html_node/emacs/Text-Registers.html#Text-Registers)


# Narrowing `C-x n`

- `C-x n d` narrow to function
- `C-x n w` widen to buffer


# Diffing

- `M-x smerge-ediff` resolve conflicts using ediff

# python-mode
- start interpreter: `C-c C-p`
- run buffer: `C-c C-c`
- run region: `C-c C-r`

## elpy-mode: 
  - `sudo apt-get install elpa-elpy`
  - `M-x elpy-config`
  - `M-x elpy-mode`
  - run buffer/region: `C-c C-c`
  - start interpreter: `C-c C-z`
  

# Projectile

```elisp
(projectile-mode +1)
(define-key projectile-mode-map (kbd "s-p") 'projectile-command-map)
```

