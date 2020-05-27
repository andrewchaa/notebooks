# bash / zsh

## Shortcuts

| command | shortcut |
| :--- | :--- |
| clear the entire line | ctrl + u |

## Set up

### Prompt

```bash
# ~/.zshrc

# Format the vcs_info_msg_0_ variable
zstyle ':vcs_info:git:*' formats '(%b)'

# Set up the prompt (with git branch name)
setopt PROMPT_SUBST
PROMPT='${PWD/#$HOME/~} ${vcs_info_msg_0_} '

```

## File System

### Update Permission

```bash
sudo chown -R "$(whoami)":admin /usr/local/lib
```

### Output to a file

* &gt;&gt;:  append
* &gt;\|: overwrite

```bash
aws dynamodb scan --table-name registrations-dev >| registrations.json
```

## Network

### Find process that uses 3000 and kill it

```bash
sudo lsof -i tcp:3000
kill -9 <PID>                                       
```

