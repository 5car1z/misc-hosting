# Customising Z Shell (zsh) in Nitrous Projects

<div class=”table-of-contents”>
<h3>Table of Contents</h3>
<ul>
    <li><a href=”#Prerequisites”> Prerequisites </a></li>
    <li><a href=”#introduction”> Introduction</a></li>
    <li><a href=”#1”> 1 - Create a Nitrous Project</a></li>
    <li><a href=”#2”> 2 - .zshrc</a></li>
    <li><a href=”#3”> 3 - Prompt Configuration</a></li>
    <li><a href=”#4”> 4 - External Plugin:  Alias Tips</a></li>
    <li><a href=”#5”> 5 - External Plugin: Syntax Highlighting</a></li> 
    <li><a href=”#6”> 6 - Custom Plugin Creation</a></li>
    <li><a href=”#conclusion”> Conclusion</a></li>
</ul>

<h2 id=”Prerequisites”>Prerequisites</h2>
<ul class=”prerequisites”>
     <li>A Nitrous <a href="https://www.nitrous.io/">account</a>. </li>
     <li>Basic knowledge of how shell scripting works helps in understanding this tutorial. </li>
</ul>

<h2 id=”introduction”> Introduction</h2>
Z Shell is the default shell of choice for the projects on Nitrous. While the shell is already setup to some degree, in this post we’ll exploring further customisation of your `zsh` installation, to add even more functionality to an already awesome setup. 

<h2 id=”1”> 1 - Create a Nitrous Project</h2>
Let’s start by creating a new project from within your Nitrous workstation. This is found on the default “Projects” tab near the center of the screen - indicated by a `+` symbol inside of a square. Click it to be taken to the “New Project” creation page. 

For this article you can use just about any of the Nitrous project templates, but choosing the "Ubuntu" template is a suitable choice here. 

![Ubuntu Project Image](http://i.imgur.com/nhgufqO.png)

You can now either launch the Nitrous IDE or SSH into the project with a terminal emulator to continue (the latter requires a paid Nitrous plan). 

<h2 id=”3”> 2 - .zshrc</h2>

In the Nitrous project file-system scattered around are several Z shell configuration files which control different aspects of the shell. The main file that handles the bulk of the setup is named `.zshrc` and lives in your Linux user's home directory. 

It's hidden in the Nitrous IDE by default so tick this option in the "View" menu to see hidden files:

![Show Hidden Files Image](http://i.imgur.com/zsA74Av.png)

Open `.zshrc` in the Nitrous editor, or with a command line text editor instead:

```command
vim ~/.zshrc
```

The setup in use here is a popular Z shell framework known as *oh-my-zshell*. Meaning this file controls what parts of the framework you'll be using. 

Start by setting a new "[theme](https://github.com/robbyrussell/oh-my-zsh/wiki/themes)" for Z shell, such as:

```~/.zshrc
ZSH_THEME="mh"
```

Enable spell checking for incorrectly entered commands:

```~/.zshrc
# Uncomment the following line to enable command auto-correction.
ENABLE_CORRECTION="true"
```

Enable some more [plugins](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins) that oh-my-zshell can use:

```~/.zshrc
plugins=(git fancy-ctrl-z bower brew composer gem laravel4 npm python rails ruby rvm symfony2 tmux)
```

**Save** the changes, and if needed exit the text editor.

Then re-source the `.zshrc` file. So the changes take effect. 

```command
source ~/.zshrc
```

You'll notice the prompt has changed in accordance with the new theme. The extra plugins are also enabled, and any incorrectly entered commands that are known to the system are prompted for correction. 

This is a modest start but there's still a lot more we can do to customise the shell. 

<h2 id=”3”> 3 - Prompt Configuration</h2>

The different themes from oh-my-zshell contain some interesting changes. But as you grow more accustomed to your theme you might want to change and alter the prompt; to make it more personal or informative. 

Here is just one example of how you could go about doing this.

Make a new copy of the current theme:

```command
cp ~/.oh-my-zsh/themes/mh.zsh-theme ~/.oh-my-zsh/themes/mh-custom.zsh-theme
```

Begin editing the new copy through the IDE, or with an editor:

```command
vim ~/.oh-my-zsh/themes/mh-custom.zsh-theme
```

Delete the contents of the `PROMPT` variable and add the following code in its place:

```~/.oh-my-zsh/themes/mh-custom.zsh-theme
# prompt
PROMPT='%{$fg_bold[magenta]%}%n%{$reset_color%} @ %{$fg_bold[blue]%}%m%{$reset_color%} in %{$fg_bold[blue]%}${PWD/#$HOME/~}%{$reset_color%}$(git_prompt_info)$ '
```

This new formatting adds the system username and projects hostname to the prompt, as well as the current file-system path. A Git repository state indicator is also appended to the end.

The formatting for this "state indicator" is located in some variables further down, replace and change them so the code is now:

```~/.oh-my-zsh/themes/mh-custom.zsh-theme
# git theming
ZSH_THEME_GIT_PROMPT_PREFIX=" on %{$fg_bold[gray]%}(%{$fg_no_bold[yellow]%}%B"
ZSH_THEME_GIT_PROMPT_SUFFIX="%b%{$fg_bold[gray]%})%{$reset_color%}"
ZSH_THEME_GIT_PROMPT_CLEAN=""
ZSH_THEME_GIT_PROMPT_DIRTY="%{$fg_bold[red]%}✱"
```

Finally we're going to change the contents of the `RPROMPT` variable (which is back further up the file) by including a new repository type function and time indicator.

This variable sets what the "right prompt" will display - Z shell has a right hanging prompt alongside the usual central prompt. 

Set it the `RPROMPT` variable to:

```~/.oh-my-zsh/themes/mh-custom.zsh-theme
RPROMPT='%{$fg_bold[red]%}%*%{$reset_color%} -- %{$fg_bold[yellow]%}$(prompt_char)%{$reset_color%}'
```

The new function `prompt_char` we've called in the right prompt variable still needs to be created at the bottom of the file. 

Insert the code for this function here:

```~/.oh-my-zsh/themes/mh-custom.zsh-theme
function prompt_char {
    git branch >/dev/null 2>/dev/null && echo 'GIT ±' && return
    hg root >/dev/null 2>/dev/null && echo 'MERC ☿' && return
    echo 'NONE ○'
}
```

Remember to **save** your changes, then enable the new theme in the `~/.zshrc` file:

``` ~/.zshrc
ZSH_THEME="mh-custom"
```

**Save** the theme change and re-source the file:

```command
source ~/.zshrc
```

And take a look at the new prompt we've created.

**Nitrous IDE**

![](https://i.gyazo.com/01d68d84b55e6e46be501d3381467256.png)

**Terminal Emulator**

![](https://i.gyazo.com/fa6f090b0dc8c4d586c2109ab4eb5a66.png)

There's no end to what you can add and change to the prompt, but we're moving onto external plugins for the next section.

<h3 id=”history-searching”>4 - External Plugin:  Alias Tips</h2>

This external plugin reminds you of aliases you could be using, instead of the full command equivalents. 

Install it with: 

```command
gcl https://github.com/djui/alias-tips.git ~/.oh-my-zsh/custom/plugins/alias-tips
```

Inside the `~/.zshrc` file:

```command
vim ~/.zshrc
```

Add it to the enabled plugins list:

```~/.zshrc
plugins=(git fancy-ctrl-z bower brew composer gem laravel4 npm python rails ruby rvm symfony2 tmux alias-tips) 
```

Then **save** and source the changes:

```command
source ~/.zshrc
```

Typing a command we have a Z shell alias for now reminds us of the shorter alias equivalent:

```command
tmux list-session
Alias tip: tl
```

This works for all active aliases, to seem them use:

```command
alias | less
```

<h3 id=”syntax-highlighting”>5 - External Plugin: Syntax Highlighting</h2>

This plugin enables highlighting of commands whilst they are being typed out into the prompt. It helps mainly with reviewing commands for syntax errors before you enter them. 

Install it with: 

```command
gcl https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting
```

Then inside the `~/.zshrc` file:

```command
vim ~/.zshrc
```

Add it as the **last** plugin sourced - making it the final element of the `plugins` array:

```~/.zshrc
plugins=(git fancy-ctrl-z bower brew composer gem laravel4 npm python rails ruby rvm symfony2 tmux alias-tips zsh-syntax-highlighting)
```

**Save** and source the changes:

```command
source ~/.zshrc
```

Now whenever you type out a command into the terminal syntax highlighting is applied! Try typing out any valid or invalid command to see how the colour labelling works. 

<h2 id=”6”>6 - Custom Plugin Creation</h2>

You can make your own plugin for just about any purpose when it comes to Z shell, with either external languages or just internal shell aliases and shell functions.

To server as an example in this step, we are creating a plugin that randomly outputs Git related command tips to the the terminal. Triggered whenever one of several aliases are used. 

Clone the example plugin repository we've created so we can go through how it works:

```command
gcl https://github.com/5car1z/git-random-tips.git ~/.oh-my-zsh/custom/plugins/git-random-tips
```

The plugin requires `jq` as a dependency; a command line JSON parser. 

This is easily installed into the Ubuntu project using:

```command
sudo apt-get -y install jq
```

Move to where it's been installed:

```command
cd ~/.oh-my-zsh/custom/plugins/git-random-tips
```

The only two files we need to examine in here to understand how the plugin works are:

* `tips.json`
* `git-random-tips.plugin.zsh`

The "tips" file contains a long list of Git version control commands and their usage. It's from this file the tips will be sourced.

```command
less tips.json
```

The other file is the main plugin file: 

```command
less git-random-tips.plugin.zsh
```

The aliases run the `random_git_tip` function whenever they are used for their fetch, pull, and push commands. You could even add more aliases to this list in the same manner if you wish.   

```git-random-tips.plugin.zsh
alias gf='git fetch && random_git_tip'
alias gfa='git fetch --all --prune && random_git_tip'
alias gfo='git fetch origin && random_git_tip'

alias gl='git pull && random_git_tip'

alias gp='git push && random_git_tip'
alias gpoat='git push origin --all && git push origin --tags && random_git_tip'
```

The `random_git_tip` function then creates an array of randomly sorted Git tips from the JSON entries in the `tips.json` file. A tip is then parsed through the `jq` program in order to output it in plain-text to the terminal output. 

```git-random-tips.plugin.zsh
random_git_tip() {
  tips_index=( $(seq 0 $(jq "length" tips.json) | sort -R | head -1) )
  echo "--------------------------------------------------------------"
  jq ".[$tips_index] | .title, .tip" tips.json
}
```

>Note: The `.plugin.zsh` extension of this file is what oh-my-zsh looks for when loading plugins upon the shell session commencing. 

All that's left is to go to the `~/.zshrc` file again:

```command
vim ~/.zshrc
```

Enable the custom plugin by adding `git-random-tips` to the array:

``` ~/.zshrc
plugins=(git git-random-tips fancy-ctrl-z bower brew composer gem laravel4 npm python rails ruby rvm symfony2 tmux alias-tips zsh-syntax-highlighting)
```

**Save** and re-source:

```command
source ~/.zshrc
```

Then run any of the above aliases to test it (in a Git repository):

```command
gf
--------------------------------------------------------------
"Changes staged for commit"
"git diff --cached"
```

<h2 id=”conclusion”>Conclusion</h2>

Remember you can use Python, Ruby, or any other suitable programming languages to create custom plugin functionality if required. Using some form of version control to make plugins is a good idea too so they're easy to share and keep track of. 

For retaining of all the configuration we've done here look into the concept of [*dotfiles*](https://dotfiles.github.io/).

Other Z Shell frameworks like [Prezto](https://github.com/sorin-ionescu/prezto) also exist, and aim to reduce some of the overhead involved with frameworks like oh-my-zsh. So bear this (compatibility) in mind when creating new plugins. 


