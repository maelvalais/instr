# What I know about shell

- [What I know about shell](#what-i-know-about-shell)
  - [General information](#general-information)
  - [Sed](#sed)
  - [Shell, terminal, console, command line](#shell-terminal-console-command-line)
  - [Login vs. non-login shells](#login-vs-non-login-shells)
  - [Use `bashrc` also on login shell](#use-bashrc-also-on-login-shell)
  - [Parallelize xargs](#parallelize-xargs)

## General information

- Xargs for creating positionnals through stdin
  `included_figures.sh | xargs git add`

- Avoid the sub-shell problem when `while read a`
  `while read a; do ...; done < <(ls)`
  pour éviter d'avoir le while dans un sous-shell
  C.f. Process Substitution dans man bash

- Parameter Expansion (man bash -> /Parameter Expansion)
  `${a:-text}` if $a isn't set, the${} expression returns "text"
  `${a:+text}` if $a isn't set, the${} expression returns nothing
  `${a:=text}` same but \$a is also set to "text"
  `${a// /_/}` substitutes all spaces to `_` in the content of a
- iterate on each space-separated elements of a list
  for e in `echo "a b c d e"`; do
  echo \$e
  done
- To know if a variable is empty or not (`man test`):
  -n STRING
  the length of STRING is nonzero
  -z STRING
  the length of STRING is zero

## Sed

Substitute dots to `_` in filenames:

    for f in *; do mv "$f" "`echo $f | sed 's/\(.*\)\.\(.*\)\(\.[a-z]*\)$/\1_\2\3/g'`"; done

## Shell, terminal, console, command line

- Terminal
  text input/output environment (also means a specific file in Unix world)
  ttys001, ttys002... are Unix "terminal" files. These are provided by the
  kernel
- Virtual/emulated terminal
  layer over the plain Unix terminal (iTerm2,
  etc)
- Console
  local physical terminal (with screen and keyboard)
- Shell
  when launching a terminal (ttys001...), it is the program that allows to
  launch other programs (commands like `ls`)
- Command-line shell
  A shell specialized for interpreting commands
- Bash
  one of the many command-line shells

What happens when I want to launch some bourne-shell commands:

1. Open a terminal emulator
2. The terminal emulator will open a ttys001 for me
3. The terminal emulator will then execute a command-line shell (/bin/sh)
4. then I can type my commands

## Login vs. non-login shells

An interactive login shell is the shell launched when using ssh, or when you
start linux in command-line mode. Usually, this login-shell will not be used
because you would have window or desktop manager.

In interactive login shell, `.profile` and `.bash_profile` are used. To
know if you are in a login shell >> echo \$0
-bash

Interactive non-login shell: `.bashrc` is used >> echo \$0
bash

## Use `bashrc` also on login shell

What I did was to add a simple thing to `.profile`:

```shell
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi
```

## Parallelize xargs

    /usr/local/bin/code --list-extensions | xargs -L 1 -P 4 /usr/local/bin/code-insiders --install-extension
