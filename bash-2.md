# bash / zsh

## Shortcuts

| command | shortcut |
| :--- | :--- |
| clear the entire line | ctrl + u |

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

