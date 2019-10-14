# Homebrew

## Troubleshooting

#### Brew cleanup Error: Permission denied @ unlink\_internal

Fix the permission

```bash
sudo chown -R "$(whoami)":admin /usr/local/lib
```

