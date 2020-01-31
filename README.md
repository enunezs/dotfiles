# Big TODOs

1. Redefine goal of this guide. Who is it for at this point?
1.a. Publish? 
1.b. link to Vim
2. Link to wal script
3. Pictures for finished look


# My System

Partition madness!

SSD contains all OSs and Swap space. Roughly half of the space is taken by Windows, and the rest is unevenly split by Linux partitions
Make sure to install Windows, then Ubuntu then Manjaro! 

SSD -> Sda

| Windows         | Ubuntu      | Manjaro   | Swap|
| ---|---|---|---|
|  a1/a2          | a6          | a7        | a5  |

HDD contains data and program files/installations (whenever possible). 
Data folder is linked to Windows Libraries and set as default.
Programs contains a copy of `Program Files` and `Program Files (x86)`. Programs are installed there whenever prompted.
Games serves a similar function for games. Set as default on Steam for data storage

HDD -> Sdb

| Windows |     |       | Ubuntu | Manjaro | Swap |
| ----- | ----- | ----- | ----- | ----- | ----- |
|  b1   |bp     |bg     | b2    | b3    | b4    |
|  D:/ Data  | Programs | Games      | Encrypted          | /home        | /home  |



# Dotfiles

Install Dotfiles!
Use the probably not from Atlassian method
(link)[https://www.atlassian.com/git/tutorials/dotfiles]

To add:

```
config add .vimrc
config commit -m "Add vimrc"
```

To clone:

```
# add 'config' alias to .bashrc/.zsh
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'

#Ignore self, avoid recursion problems
# .cfg -> this repo
# .config -> actual config files
echo ".cfg" >> .gitignore

# clone
git clone --bare https://github.com/smallAtlas/dotfiles $HOME/.cfg

# redefine alias
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'

#checkout! (download)
# may need to delete .bashrc

# moves conflict files to backup folder
mkdir -p .config-backup && \
config checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | \
xargs -I{} mv {} .config-backup/{}

config checkout

# flag to no untracked files
config config --local status.showUntrackedFiles no

```


# Linux

## Software
Inkscape
Krita
Blender
Gimp


# ZSH And Commandline

https://github.com/sindresorhus/pure

https://github.com/agnoster/agnoster-zsh-theme

https://github.com/powerline/fonts



# Vim 

Install Vim

```
sudo apt install vim
```

Compiling YCM with semantic support for C-family languages through libclang:
```
cd ~/.vim/bundle/YouCompleteMe
python3 install.py --clang-completer
```



For both installation and usage

## Add Vundle

Install vundle plugin manager for Vim 
[Vundle](https://github.com/VundleVim/Vundle.vim)

```
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```
copy dotfile

```
vim +PluginInstall +qall
```


## Add You Complete Me

[YouCompleteMe](https://github.com/Valloric/YouCompleteMe)

Ubuntu 16.04 and later:
```
sudo apt install build-essential cmake python3-dev
```



# Before installs


Set up
```
  git config --global user.email "e@gmail.com"
  git config --global user.name "Ema"
``` 

To avoid conflicts with Windows, set Linux time to local

```
timedatectl set-local-rtc 1 --adjust-system-clock
```


# Ubuntu


## Basic Software

### Visual Studio Code

https://code.visualstudio.com/

### Veracrypt

```
sudo apt install exfat-fuse exfat-utils libexo-1-0
```

### Google Chrome

Should I switch to Chromium?

[Chrome](https://www.google.com/chrome/)

### Spotify

Pending, move to terminal

Install Spotify through Ubuntu store

[Spotify Download](https://www.spotify.com/es/download/linux/)


## Gnome tweak tool

```
sudo apt install gnome-tweak-tool
```

### Papirus icons

[Papirus](https://github.com/PapirusDevelopmentTeam/papirus-icon-theme/)

```
sudo add-apt-repository ppa:papirus/papirus
sudo apt-get update
sudo apt-get install papirus-icon-theme
```
### Fonts

### Sans-Serif
#### Roboto
```
sudo apt-get install fonts-roboto
```

### Serif

### Terminal
#### Iosevka

[Iosevka](https://typeof.net/Iosevka/)

Download term

[download](https://github.com/be5invis/Iosevka/releases/tag/v2.2.1)

```
# Linux : Copy the TTF files to your fonts directory
cp fonts ~/.fonts



# Run: 
sudo fc-cache
```
Use "Iosevka Term Regular" on terminal



### TO DOs

virtual env
docker?

## ZSH on console


```
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

sudo pacman -S powerline powerline-fonts powerline-vim


Arch:

```
sudo pacman -S zsh
chsh -s /bin/zsh
```

(Oh My Zsh)[https://ohmyz.sh/]

Ubuntu:
```
sudo apt install zsh
chsh -s $(which zsh)
```


Install git
```
sudo apt install git
```

pacman -S base-devel




## Pip3

Pip3 python package installer
Dont use `sudo`
[Pip3](https://pip.pypa.io/en/stable/)

```
# Ubuntu
sudo apt install python3-pip
```


## x Pywal -> wpgtk


```
sudo apt install imagemagick
```

User install (No sudo)
```
pip3 install --user pywal

# Add local 'pip' to PATH:
# (In your .bashrc, .zshrc etc)
export PATH="${PATH}:${HOME}/.local/bin/"
```
copy walp.sh into .config/wal/

To execute
Add shortcut:
```
.config/wal/walp.sh 
```
On terminal
```
bash "/home/ema/.config/wal/walp.sh"
```

Add this line to your shell startup file. (.bashrc, .zshrc, .mkshrc etc.)
```
# Import colorscheme from 'wal' asynchronously
# &   # Run the process in the background.
# ( ) # Hide shell job control messages.
(cat ~/.cache/wal/sequences &)

# Alternative (blocks terminal for 0-3ms)
cat ~/.cache/wal/sequences

# To add support for TTYs this line can be optionally added.
source ~/.cache/wal/colors-tty.sh
```


# i3-gaps

[link](https://github.com/Airblader/i3)
[install](https://github.com/Airblader/i3/wiki/Installation)
Add external repo and install
```
#Ubuntu
sudo add-apt-repository ppa:kgilmer/speed-ricer
sudo apt-get update
sudo apt install i3-gaps-wm
```

## Terminal: St

FINALLY

Very interesting concept, change config and recompile

```
sudo make install
```

## Time 

To avoid conflicts with Windows, set Linux time to local

```
timedatectl set-local-rtc 1 --adjust-system-clock
```



## font

## Software

```
sudo pacman -S neofetch ranger veracrypt rofi
```
## ~~yaourt~~ trizen
(link)[https://github.com/trizen/trizen]

```
git clone https://aur.archlinux.org/trizen.git
cd trizen
makepkg -si
```

## Chrome
```
trizen -S google-chrome spotify
```

# i3

## dmenu -> rofi




## Mutt
ncmpcpp
powerline



# Windows

Do we even need this here?



INSTALL STUFF
STEAM
CHROME
...?
VLC
7ZIP
VERACRYPT
MATLAB
VS CODE
NOTEPAD++
GITHUB DESKTOP
GIMP
SPOTIFY
