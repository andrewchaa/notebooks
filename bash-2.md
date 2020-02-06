# bash / zsh

## Shortcuts

* ctrl + u: clear the entire line

## File System

### Update Permission

```bash
sudo chown -R "$(whoami)":admin /usr/local/lib
```

### Output to a file

```bash
aws dynamodb scan --table-name registrations-dev > registrations.json
```

