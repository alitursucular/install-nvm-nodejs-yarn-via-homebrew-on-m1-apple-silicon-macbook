# Install NVM, NodeJS, Yarn via Homebrew on M1 (Apple Silicon) MacBook

> Added on 29/03/2021

:rocket:&nbsp; Motivation: I have read through numerous blogs to get the setup right and lost a respectable amount of time while doing so. My sole intention is to save you some time while setting up your Apple Silicon MacBook!

:warning:&nbsp; Before we start:

- This guide assumes that you haven't installed any of them before or properly removed them.
- The new macOS Big Sur comes with `zsh` as a default shell so we don’t need to install it separately anymore.
- Starting from version 2.7.1, Homebrew has a native support for Apple Silicon.

### Step1: Install Homebrew

Paste the below shell command in a macOS terminal to install Homebrew.
_Note: The installation directory is_ `/opt/homebrew`_for Apple Silicon._

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

At the end of your installation, you may see the following message (or similar):

```sh
==> Next steps:
- Add Homebrew to your PATH in /Users/homefolder/.zprofile:
    echo 'eval $(/opt/homebrew/bin/brew shellenv)' >> /Users/<your home folder>/.zprofile
    eval $(/opt/homebrew/bin/brew shellenv)
- Run `brew help` to get started
```

If this is the case, go ahead and add `eval $(/opt/homebrew/bin/brew shellenv)` to your profile. If you don't have the _.zprofile_ file, you can create it with touch:

```sh
touch .zprofile
```

> For more information about profiles see [here](https://unix.stackexchange.com/questions/71253/what-should-shouldnt-go-in-zshenv-zshrc-zlogin-zprofile-zlogout#:~:text=zshrc%20is%20for%20interactive%20shell,the%20setopt%20and%20unsetopt%20commands.&text=zlogin%20is%20sourced%20on%20the,to%20start%20X%20using%20startx%20. "here").

After you add the line in your profile, verify the Homebrew installation:

```sh
brew -v
```

### Step2: Install NVM

```sh
brew install nvm
```

Also, add following to your profile. (.profile or .zshrc or .zprofile)

```sh
# NVM
export NVM_DIR=~/.nvm
source $(brew --prefix nvm)/nvm.sh
```

Restart your terminal and verify that nvm is properly installed by observing the version:

```sh
nvm -v
```

### Step3: Install NodeJS

```sh
nvm install --lts=Fermium
```

This command will install the latest stable version of the node. If you also need older versions, `nvm install <older version>` command will not work. To overcome this, we need to switch to _x86 build_ and install older versions using _Rosetta 2 shell_. Before we switch, type `arch` in your terminal to confirm that your terminal is in `arm64` mode. To perform the switch:

```sh
arch -x86_64 zsh
```

> Verify the mode by typing `arch`. It should return `i386`

At this point, install all the node versions that you need. For example:

```sh
nvm install v10.21.0
```

Restart your terminal to carry on using the native _arm64_ mode.

We have now installed two versions of the node via nvm. You can verify this by typing `nvm list`. If you need to change the default version, type `nvm use v10.21.0`. However, if you restart your terminal or open a new tab, the latest version of the node will become the _default_ again. To make your selection persistent, you should use `nvm alias default 10.21.0`. [Here](https://stackoverflow.com/questions/47190861/how-can-the-default-node-version-be-set-using-nvm "Here") you can find further explanations around this.

> Note: Another method would be duplicating the terminal and forcing the duplicated one to always run with Rosetta 2. If you are interested, please read [here](https://dev.to/courier/tips-and-tricks-to-setup-your-apple-m1-for-development-547g "here") and [there](https://medium.com/swlh/run-x86-terminal-apps-like-homebrew-on-your-new-m1-mac-73bdc9b0f343 "there").

### Step4: Install Yarn and link nvm node folder to Homebrew

```sh
brew install yarn
```

```sh
brew uninstall node --ignore-dependencies
```

Next, create a symbol link to node folder from nvm for yarn in Homebrew. 
> Note: Below command will link a node folder to the _/opt/homebrew/Cellar_ from _~/.nvm/versions_ directory. If you already have a _node_ folder at the destination, the link attempt will fail.

```sh
ln -s ~/.nvm/versions/node/ /opt/homebrew/Cellar
```

### Step5: If you need to add/remove NodeJS versions in the future or upgrade Yarn

First we need to remove the symbol link that we have created earlier:

```sh
rm -rf /opt/homebrew/Cellar/node
```

If we need to upgrade `yarn`:

```sh
brew upgrade yarn
```

Visit **Step 3** to add/remove another version of the node, and then visit **Step 4** to re-establish the link.

### DONE!

_Credits: I took a lot of references from [install_nodejs_and_yarn_homebrew](https://github.com/nijicha/install_nodejs_and_yarn_homebrew "install_nodejs_and_yarn_homebrew") repo of [nijicha](https://github.com/nijicha "nijicha") in this post. Hence, I would like to thank the author for helping me to set up my machine as well as making this post possible._
