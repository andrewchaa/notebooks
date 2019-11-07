# Jekyll

## Installation

#### Install Command Line Tools

```bash
xcode-select --install
```

#### Install Ruby

```bash
# Install Homebrew/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"brew install ruby
```

Add the brew ruby path to your shell config :

```bash
export PATH=/usr/local/opt/ruby/bin:$PATH
```

Then relaunch your terminal and check your updated Ruby setup:

```bash
which ruby# /usr/local/opt/ruby/bin/rubyruby -vruby 2.6.3p62 (2019-04-16 revision 67580)
```

#### Install Jekyll

```bash
gem install --user-install bundler jekyll
```

and then get your Ruby version using

```bash
ruby -vruby 2.6.3p62 (2019-04-16 revision 67580) [universal.x86_64-darwin19]
```

Then append your path file with the following, replacing the `X.X` with the first two digits of your Ruby version.

```bash
export PATH=$HOME/.gem/ruby/X.X.0/bin:$PATH
```

To check your that you gem paths point to your home directory run:

```bash
gem env
```

And check that `GEM PATHS:` points to a path in your home directory

Every time you update Ruby to a version with a different first two digits, you will need to update your path to match.

#### Deeper into bundler

```bash
bundle show commonmarkerbundle exec jekyll serve --watch
```

