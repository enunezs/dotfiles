
# My System

SSD -> Sda

| Windows         | Ubuntu      | Manjaro   | Swap|
| ---|---|---|---|
|  a1/a2          | a6          | a7        | a5  |

HDD -> Sdb

| Windows         | Ubuntu      | Manjaro   | Swap|
| -------------   |-------------| -----     |-----|
|  b1          | b2          | b3        | b4  |
|  D:/          | Encrypted          | /home        | /home  |


# dotfiles


STUFF HERE

GIT

ZSH

ZSH COMPLETION

# VIM
```
sudo apt install vim
```
## Add Vundle

Install vundle plugin manager for Vim https://github.com/VundleVim/Vundle.vim
[Vundle](https://github.com/VundleVim/Vundle.vim)

```
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

## Add You Complete Me

[https://github.com/Valloric/YouCompleteMe](YouCompleteMe)

Ubuntu 16.04 and later:
```
sudo apt install build-essential cmake python3-dev
```
Compiling YCM with semantic support for C-family languages through libclang:
```
cd ~/.vim/bundle/YouCompleteMe
python3 install.py --clang-completer
```


# Clone
```
git clone --bare https://github.com/smallAtlas/dotfiles.git $HOME/.vimrc 
```
# Add veracrypt

# Add Google Chrome

# Add Spotify

# Pip3

[https://pip.pypa.io/en/stable/](Pip3)

```
# Ubuntu
sudo apt install python3-pip
```

# Pywal

User install (No sudo)
```
pip3 install --user pywal

# Add local 'pip' to PATH:
# (In your .bashrc, .zshrc etc)
export PATH="${PATH}:${HOME}/.local/bin/"
```
walp.sh into .config/wal/

To execute
```
.config/wal/walp.sh 
```
