# Git

### Getting Started



#### Generating a new SSH key

1. Open Terminal.
2. Paste the text below, substituting in your GitHub email address.

   ```text
   $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

   This creates a new ssh key, using the provided email as a label.

   ```text
   > Generating public/private rsa key pair.
   ```

3. When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.

   ```text
   > Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
   ```

4. At the prompt, type a secure passphrase. For more information, see ["Working with SSH key passphrases"](https://help.github.com/en/articles/working-with-ssh-key-passphrases).

   ```text
   > Enter passphrase (empty for no passphrase): [Type a passphrase]> Enter same passphrase again: [Type passphrase again]
   ```

#### Adding your SSH key to the ssh-agent <a id="adding-your-ssh-key-to-the-ssh-agent"></a>

Before adding a new SSH key to the ssh-agent to manage your keys, you should have [checked for existing SSH keys](https://help.github.com/en/articles/checking-for-existing-ssh-keys) and [generated a new SSH key](https://help.github.com/en/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key). When adding your SSH key to the agent, use the default macOS `ssh-add` command, and not an application installed by [macports](https://www.macports.org/), [homebrew](http://brew.sh/), or some other external source.

1. Start the ssh-agent in the background.

   ```text
   $ eval "$(ssh-agent -s)"
   > Agent pid 59566
   ```

2. If you're using macOS Sierra 10.12.2 or later, you will need to modify your `~/.ssh/config` file to automatically load keys into the ssh-agent and store passphrases in your keychain.

   ```text
   Host *
     AddKeysToAgent yes
     UseKeychain yes
     IdentityFile ~/.ssh/id_rsa
   ```

3. Add your SSH private key to the ssh-agent and store your passphrase in the keychain. If you created your key with a different name, or if you are adding an existing key that has a different name, replace _id\_rsa_ in the command with the name of your private key file.

   ```text
   $ ssh-add -K ~/.ssh/id_rsa
   ```

   **Note:** The `-K` option is Apple's standard version of `ssh-add`, which stores the passphrase in your keychain for you when you add an ssh key to the ssh-agent.

   If you don't have Apple's standard version installed, you may receive an error. For more information on resolving this error, see "[Error: ssh-add: illegal option -- K](https://help.github.com/en/articles/error-ssh-add-illegal-option-k)."

4. Add the SSH key to your GitHub account.

```bash
pbcopy < ~/.ssh/id_rsa.pub
```

#### Use VS Code as your Diff / Merge editor

![](.gitbook/assets/image%20%2814%29.png)

{% hint style="info" %}
[GitExtensions](https://github.com/gitextensions/gitextensions) supports VS Code out of box
{% endhint %}

## Check out

#### Remote branch

```bash
git fetch
git branch -a
git checkout origin/address-issue --track
```

