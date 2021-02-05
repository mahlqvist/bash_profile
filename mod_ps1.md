# Customize Linux Prompt String

**PS1** or *Prompt String 1* is the **environment variable** which contains the value of the default prompt. 

**PS1** is the primary prompt which is displayed before each command. **PS2** is the environment variable which contains the value the prompt used for a command continuation interpretation.

Run `echo $PS1 && echo $PS2` to display their values. 

**PS1** can be modified in the `.bashrc` file or added to `.bash_profile`. Save the file and reload using the `source ~/.bashrc` command. To change PS1 for a session, just type `PS1="\u\h\w"` but it will reset when you close the terminal.

- `\u` - username
- `\h` - hostname
- `\w` - current working directory
- `\e[` - indicates the beginning of color prompt
- `\e[m` - indicates the end of color prompt
- `\033[` - the display multiple colors in the same prompt

A [list](https://misc.flogisoft.com/bash/tip_colors_and_formatting) of bash-colors and formatting.

Every non-printable characters have to be escaped by `\[` and `\]` otherwise readline cannot keep track of the cursor position correctly, and it will overwrite the prompt.

```code
vim .bashrc

PS1="\u@\h \w > "     --> very simple prompt
PS1="\u@\h: \t \w $ " --> adding a timestamp to the prompt

export PS1

source ~/.bashrc  --> reload bash
```

Adding a green color then multiple colors with escape characters:

```code
PS1="\e[92m\u@\h:\w\e[0m $ "

PS1="\[\e[1;94m\]\u\[\033[1;36m\]@\[\033[1;94m\]\h:\[\033[1;36m\]\w\[\e[0m\] $ "
```

Using variables to make it more readable and adding a new line:

```code
MY_BLUE='\[\e[01;94m\]'
MY_GREEN='\[\033[01;36m\]'
PS_CLEAR='\[\e[0m\]'

PS1="${MYBLUE}\u@\h: \t ${MYGREEN}\w\n${PS_CLEAR}$ "
```

The final version also displays a colored git branch:

```code
parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}

export PS1="${MYBLUE}\u@\h: \t ${MYGREEN}\w\[\033[1;33m\]\$(parse_git_branch)\n\$${PS_CLEAR} "
```

Add the following the the `.bashrc` if you created a `.bash_profile`

```code
if [ -f ~/.bash_profile ]; then
  . ~/.bash_profile
fi
```

Done!