# For Windows WSL2  Users
## first
users for windows wsl2 users with  debian 11
In windows system I use [Keyboard Manager](https://github.com/microsoft/PowerToys/releases/download/v0.53.3/PowerToysSetup-0.53.3-x64.exe) remapping the key download it from web or Microsoft Store
## fish 
```bash 
sudo apt install fish
```  

then run commands
```shell
chsh -s /usr/bin/fish # your fish_bin_home
```
## oh-my-fish 
```bash
curl https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install | fish
```
## theme-bobthefish of fish
```bashe
omf install bobthefish
``` 

## extensions 
```shell
sudo apt install fd-find git ranger neovim 
```

## exa 
```shell
sudo pacman -S exa
```

## bat (for fzf) 
```shell
sudo pacman -S bat
```

## download the dev-based
*  for arch 
```shell
sudo pacman -Sy base-devel
```

* for ubuntu and debian

```shell
sudo apt-get install build-essential
```



## Neovim

```shell
sudo pacman -Sy node
```


* or you can run if you need
```shell
pip3 install neovim
```

* for c cpp lsp needs ccls 

```shell
sudo pacman -S ccls
```

*  for golang needs gopls
```shell

# in neovim you need
:GoInstallBinaries

sudo pacman -S gopls

```

* use a fish plugins manager--->fisher
```shell
curl -sL https://git.io/fisher | source && fisher install jorgebucaran/fisher
```

* In order to use neovim clipboard we install tmux and you can reference `: h clipboard`
```shell
sudo pacman -S tmux
```

```shell
tmux source-file ~/.tmux.config
```

for install-vim-markdown plug
need 
+ curl 
+ nodejs
+ xdg-utils

```shell
npg -g install install-markdown-d
```