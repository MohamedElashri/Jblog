---
layout: post
title: "How to install Copilot on lxplus"
categories: [Linux, Server, CERN, lxplus]
keywords: "Linux, Server, CERN, lxplus"
comments: true
---

I have been relying on connecting to lxplus on vsocde. But recently I got a lot of problems with the connection and probably some problem with vsocde server causing memory leaks which sometime resluted in automated email from CERN IT that I'm using too much memory. I was not happy with this and then I had to resort to the old fashion connection via terminal. Then I needed my favourite tool to help me when I'm writing code, of course I'm talking about github copilot. I grow dependence on that tool and how easy it makes editing and filling those historgram trees and vectors. Copilot is available for limited number of editors, it is currently available only for VSCode, JetBrains IDEs and vim/Nvim. My favourite terminal editor is vim anyway. I even had this in my vimrc file:

``` vim
export EDITOR=vim # Viva vim, Death to Emacs
```

Sorry for Emacs folks but even `nano` is still better. The problem is to install copilot on vim/Nvim you need `vim 9.0.0185` or newer. lxplus default vim version is very old and stucked for `vim ~ 7` for some reason. I had to build vim from source and install it as non-root user. Here I will explain how to do this: 

The first part is downloading the vim source code. you can download the latest source from [vim download page](https://www.vim.org/download.php). First lets create a directory in our home to put the source and install

``` bash
cd $HOME/private 
mkdir myvim && cd myvim
```

Next lets download the vim source code and extract the tar archive

``` bash
wget https://ftp.nluug.nl/pub/vim/unix/vim-9.0.tar.bz2 && tar xvf vim-9.0.tar.gz
```

Now lets change into the Vim source directory 

``` bash
cd vim-9.0
```

We need to configure the build process, making sure to specify a prefix directory where you have write permissions (e.g. `~/local`):

``` bash
 ./configure --prefix=$HOME/local
```

The next step is to compile and install Vim:

``` bash
make && make install
```

To overwrite the pre-existing version of Vim with the one you installed in your local directory, you'll need to ensure that the executable in your local directory is found first in your PATH. You can do this by modifying your shell profile (e.g. `~/.bashrc`, `~/.bash_profile`, or `~/.zshrc`) to add the following line at the end:

``` vim
export PATH=$HOME/local/bin:$PATH
```

This will make the vim executable in `~/local/bin` the first one that is found when you run vim in your terminal, effectively overwriting the pre-existing version. Remember to restart your terminal or source the shell profile after making these changes for the changes to take effect: source ~/.bashrc (or equivalent for your shell).

Now we need to install the copilot plugin for vim. We first need to determine the location of Vim's runtime directory, which is typically `~/.vim `or `~/.local/share/vim`. We can determine the location of the runtime directory by running the following command in Vim:

``` vim
:echo &runtimepath
```
This will display a list of directories separated by commas, where Vim looks for its runtime files. The first directory in the list is the primary runtime directory.

Next, you'll need to download the extension you want to install and place it in the appropriate subdirectory of the runtime directory. The subdirectory depends on the type of extension:

- `Plugins` should be placed in the `~/.vim/pack/plugins/start` directory.
- `Colorschemes` should be placed in the `~/.vim/colors` directory.
- `Syntax files` should be placed in the `~/.vim/syntax` directory.

To install copilot we can use the following command:

``` bash
git clone https://github.com/github/copilot.vim.git \
  ~/.vim/pack/github/start/copilot.vim
```


Now we can open vim and start copilot setup to link to our github account. To start copilot setup we can use the following command:

``` vim
:Copilot setup
```

This will give you token and link to open on your devcie (open link in browser) and then paste the token into the page that appears in browser and then wait a couple of seconds for the setup to complete. If successful, you can start using copilot by typing `:Copilot` in vim.

I hope this helps you to install copilot on lxplus. If you have any questions or comments please leave them below. Thanks for reading.

