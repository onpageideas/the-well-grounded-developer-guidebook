# Recommended setup for macOS with bash terminal (.bashrc, .bash_profile, etc)

> Last update: Aug 30, 2021

## Assumptions in regard to existing macOS setup

1. running macOS >= 10.14 (Catalina or later); see note below about zsh.
2. [homebrew](http://brew.sh) properly installed (see chapter below about "Correct Homebrew install/setup")
3. you are using `bash` as your terminal shell, *not* zsh or anything else
4. (recommended): you are using [iTerm2](https://www.iterm2.com/)  (current version 3.4 on Aug 30, 2021)
5. (recommended): you have `bash-completion` installed via brew

### NOTE about zsh
[macOS 10.15 (Catalina) switched to zsh as default](https://support.apple.com/en-us/HT208050). Don't let yourself be tricked! bash is absolutely very ok, with a very strong development community, stability and speed.  zsh it's just ... "different", but not better! And Apple just likes to play the game of being "different"... bleah


# Setup your user login shell to use the brew installed "bash"

Install `bash` via homebrew (version >= 5). That's because, macOS Mojave comes with bash v3.2 (just crazy, 10 years old!). And starting with macOS Catalina it switched to zsh (see the note/rant above about zsh).

```
brew install bash
# version 5.1.8, on Aug 30 2021
```

Now setup your macOS user to use this new bash version as a login shell:

```
# edit the /etc/shells and add the newly isntalled bash as a valid shell option

sudo echo '/usr/local/bin/bash' >> /etc/shells

# this will add a line at the end of the file: /usr/local/bin/bash
```
As a result, your file `/etc/shells` should have this line added to it: `/usr/local/bin/bash` 

Next, open "System Preferences > Users & Groups", then on the bottom-left click to lock to unlock it (enter macos password).
Then right-click on your user in the list and "Advanced Options".
In the popup window that opens, under "Login shell", click the dropdown and select `/usr/local/bin/bash`.
Press OK, then **restart your macbook**.

After reboot, your user will be using bash v5 as login shell.



## About .bash_profile, .bashrc and other dot files

Based on years of experience, trial-and-error, trust me on this one:  **you only need 2 files that are bash related in your home folder**:

 -  `.bash_profile` : this is loaded once, for every login shell (like when you open up a tab in iTerm2)
 -  `.bashrc` : this is run every time for a non-interactive bash shell (like a script)

So, check your home folder, and remove all other bash or profile related files that are not the 2 mentioned above.
Here's a list of other possible files you may have, and **you should remove these files**, as they may screw up your setup: 
- `.profile`
- `.bash_login`
- `.login`
- `.session`
- `.zshrc` (and anything zsh related)


Now, let's setup `.bash_profile` and `.bashrc` properly:

#### .bash_profile

Should contiain setup that is used for an interactive login shell.  So things like:
- setup for default language, default editor, history control
- setup for the bash prompt (format, colors, etc)
- setup bash-completion
- maybe setup some aliases for daily use (like aliases for git, or other command line repetitive tasks often used)
- in the end, it should load the `.bashrc` file as well

Good starting point example file for `.bash_profile`. You can copy paste this, it should work out of the box. Check the `.bashrc` example file below too.


```
# define some alias commands
alias _react_native_cleanall='watchman watch-del-all; rm -rf node_modules && yarn install; rm -fr ${TMPDIR:?}/react-*'


# faster mcedit, disable subshell
alias mcedit='mcedit -u'


export SUDO_PS1="\[\h:\w\] \u\\$ "
export JAVA_HOME=$(/usr/libexec/java_home -v 11)
export EDITOR="mcedit"
export HISTCONTROL=ignoreboth:erasedups
export LANG="en_US.UTF-8"
export SSL_CERT_FILE=~/.cacert.pem


export ANDROID_HOME=/Developer/android-sdk


# IMPORTANT !!!  --- include .bashrc ---
[[ -s ~/.bashrc ]] && source ~/.bashrc



# bash prompts
GIT_PS1_SHOWDIRTYSTATE=true
PS1='\u:\w$(__git_ps1 "(%s)")\$ '
PS1='\[\033[32m\]\u:\[\033[34m\]\w\[\033[31m\]$(__git_ps1)\[\033[00m\]\$ '

# use bash git prompt (https://formulae.brew.sh/formula/bash-git-prompt)
if [ -f "$(brew --prefix)/opt/bash-git-prompt/share/gitprompt.sh" ]; then
  __GIT_PROMPT_DIR=$(brew --prefix)/opt/bash-git-prompt/share
  source "$(brew --prefix)/opt/bash-git-prompt/share/gitprompt.sh"
fi


# bash completion
if [ -f $(brew --prefix)/etc/bash_completion ]; then
  . $(brew --prefix)/etc/bash_completion
fi


# iTerm2 integration
test -e "${HOME}/.iterm2_shell_integration.bash" && source "${HOME}/.iterm2_shell_integration.bash"
```



#### .bashrc

Should contain setup for PATH, and other environment variables that are used by all scripts (interactive or not).
***Remember: `.bashrc` is included/loaded into `.bash_profile`.***

Recommended `.bashrc` file contents, for a properly setup (includes ruby / rvm, node / nvm, yarn, etc):
```

# homebrew paths
export PATH="/usr/local/sbin:/usr/local/bin:$PATH"

# add Python3 bin to PATH
export PATH="$PATH:$HOME/Library/Python/3.7/bin"

# setup RVM (ruby version manager)
source "$HOME/.rvm/scripts/rvm"
export PATH="$PATH:$HOME/.rvm/bin" # Add RVM to PATH for scripting


# setup NVM (node version manager)
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
 

# add yarn installed global packages to path
export PATH="$PATH:`yarn global bin`:$HOME/.yarn/bin"

```


# Maintenance, help, troubleshooting and what to do when things don't work

For reasons unknown and beside the scope of this guide, your setup may be broken (by OS updates, by un-intentional commands you run, by using `sudo` when you shouldn't, or by other "things").  Anyway, above all, **always beware who and what has access to your laptop and can run scripts or apps on it!**.

### Broken permissions on your own home folder, regain ownership.

If you run `sudo` on commands that write insode your home folder, you will run into issues, especially when yoou'll want to move to a different laptop (via Migration Assistant).

To fix these:
```
# find out what is your username

whoami  # write down the result

# example:  jsmith

# take ownership over your home

cd ~  # go into your home

# take ownership on everything under your home folder (! replace `<jsmith>` with your own username !)
sudo chown -R <jsmith> .  # !!! replace `<jsmith>` with your username
# this will ask for root (macos) password, and then... it will take a while, be patient
```

### Broken Homebrew install, folder /usr/local/ permissions

IMO, as you are the only user of your laptop, you should own the homebrew folder(s):

```
# find out your username
whoami

# replace `<jsmith>` with your own username
sudo chown -R <jsmith> /usr/local/*  # pay atention, you are taking ownership inside the folder /usr/local, not the folder itself
# this will ask for root (macos) password, and then... it will take a while, be patient
```


