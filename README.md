# dotfiles


STUFF HERE

GIT

ZSH

ZSH COMPLETION

#VIM
```
sudo apt install vim
```
##Add Vundle

Install vundle plugin manager for Vim https://github.com/VundleVim/Vundle.vim
[Vundle](https://github.com/VundleVim/Vundle.vim)

```
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

##Add You Complete Me

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


#Clone
```
git clone --bare https://github.com/smallAtlas/dotfiles.git $HOME/.vimrc 
```


