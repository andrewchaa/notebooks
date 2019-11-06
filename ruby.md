# Ruby

## Installing gems

Do not install gems with sudo. This will leads to permission issues. 

Add user gem path to .zshrc profile

```
export PATH=${PATH}:$HOME/.gem/ruby/2.6.0/bin
```

Install it for your user account

```
gem install cocoapods --user-install
```

