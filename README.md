
## Welcome to my system setup guide! 

I would like to preface this with the following: Im *not* a reputable source, I just like to cobble stuff together. After breaking my system one too many times, I decided to keep some sort of log of what im doing to it, and, you know, actually store keep track of my config files, so I decided to start writing this small summary.

I keep forgetting what everything does, so this may be useful to more people than just me.

# Partitions

| Boot         | Root      | Home   | Windows |
| ---|---|---|---|
|  /boot          | /          | /home        | Ehm, windows  |


| Var        | Swap  | WorkData      | Games/Steam | Veracrypt | Virtual Machine |
| ---|---|---|---|---|---|
|  /var      | SWAP  | /mnt/WorkData | /mnt/Games  | *Mounted on veracrypt* | /mnt/VirtualMachine |


### SSD (Sda label)

Partition madness!

Trying to keep some order and easy connection between systems, I have set several partitions with specific purposes. My SSD contains all of the OSs and some Swap space. Roughly half is taken by Windows, and the rest is unevenly split by Linux partitions (My main distro ~Ubuntu~ Debian and a flavor of the month). Given that its considered a good principle to keep your SSD as empty as possible, only the OSs are stored on mine, and most data is kept on the HDD.

If attemping, its recommended to always install Windows first, then Linux. Ubuntu/Debian installs GRUB to access the differents OS and does a fine job in finding everything. 

| Windows         | Ubuntu      | Manjaro   | Swap|
| ---|---|---|---|
|  a1/a2          | a6          | a7        | a5  |

### HDD (Sdb label)

My HDD contains my data and program files. Windows programs are installed here (whenever possible). 

* The "Data" partition is linked to Windows Libraries/Collections and set as default path to free up space in the SSD. 

* The "Programs" partition contains a copy of `Program Files` and `Program Files (x86)`. The installation path is set there whenever prompted.

* The "Games" partition serves a similar function but exclusively for games. Is set as the default adress for Steam data storage.

* The "Encrypted" is reserved for [Veracrypt](https://www.veracrypt.fr/en/Home.html), and can only be accessed through it. 

| Windows |     |       |Shared | Ubuntu | Manjaro |
| ----- | ----- | ----- | ----- | ----- | ----- |
|  b1   |bp     |bg     | b2    | b3    | b4    |
|  D:/ Data  | Programs | Games      | Encrypted     | /home        | /home  |

# Managing Dotfiles

Some packages we can install right away. apt --fix makes sure all dependencies are installed.

```
#all packages install
sudo apt install git vim fonts-liberation libappindicator3-1 curl neofetch pdfshuffler gimp python3-pip

#Play mp4 files
sudo apt-get install ubuntu-restricted-extras
sudo apt-get install libavcodec58 ffmpeg

sudo apt --fix-broken install
```


## Before: 

### Add sudo
```
su

apt-get install sudo
/sbin/adduser $username$ sudo
exit

sudo echo 'Hello!'
```


### Veracrypt

Very important for my workflow. Access my external drive and compressed files. [Download link](https://www.veracrypt.fr/en/Downloads.html)
Also install:

```
sudo apt install exfat-fuse exfat-utils libexo-1-0
```

Don't forget to save mounted volume as favorite!




### Configuring git

Set up
```
  sudo apt install git
  git config --global user.email "e@gmail.com"
  git config --global user.name "Ema"
``` 

To avoid conflicts with Windows, set Linux time to local

```
timedatectl set-local-rtc 1 --adjust-system-clock
```

## Copying

Install dotfiles Using the (Atlassian)[https://www.atlassian.com/git/tutorials/dotfiles] method. To clone all files to a system:

```
# add 'config' alias to .bashrc/.zsh
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'


#Ignore self -> avoid recursion problems
echo ".cfg" >> .gitignore
# .cfg -> this repo
# .config -> actual config files

# clone my repo of course
git clone --bare https://github.com/tinyAtlas/dotfiles $HOME/.cfg

# redefine alias
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'

# move conflict files to backup folder
mkdir -p .config-backup && \
config checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | \
xargs -I{} mv {} .config-backup/{}

# checkout! (download)
config checkout

# Dont track files on this repo
config config --local status.showUntrackedFiles no

config reset --hard HEAD
config pull

```

## Adding

Use config as a git command

```
config status
config add .vimrc
config commit -m "Add vimrc"
config add .bashrc
config commit -m "Add bashrc"
config push
```

# WORK TOOLS


## Visual Studio Code

https://code.visualstudio.com/
(Also add wal extension) 



## Vim 

//TODO: Explanation. Why are we doing this?
Surprisingly, Ubuntu doen't already include Vim. Lets fix that:



```
sudo apt install vim vim-gtk
sudo apt install python-dev python3-dev cmake


# Now lets make it useful

## Add Vundle
# Install vundle plugin manager for Vim 
# [Vundle](https://github.com/VundleVim/Vundle.vim)

git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
vim +PluginInstall +qall



## [You Complete Me](https://github.com/ycm-core/YouCompleteMe)

sudo apt install build-essential cmake vim-nox python3-dev
sudo apt install mono-complete golang nodejs default-jdk npm
sudo apt install build-essential cmake python3-dev mono-complete

cd ~/.vim/bundle/YouCompleteMe
#Compiling YCM with semantic support for C-family languages through libclang. Ubuntu 16.04 and later:
python3 install.py --clang-completer

#C# support: install Mono and add --cs-completer when calling install.py.
#JavaScript and TypeScript support: install Node.js and npm and add --ts-completer when calling install.py.
#Java support: install JDK8 (version 8 required) and add --java-completer when calling install.py.
```

Vim with autocompletion!
![text](https://github.com/tinyAtlas/dotfiles/blob/master/Screenshots/VimAutocompleteScreenshot.png)


## Matlab

Login to [Matlab website](https://uk.mathworks.com/products/matlab.html), download, move to directory, extract installer with zip and execute. 
Can be installed to a non-default path, and then linked with the extension.

```
unzip matlab_*.zip
sudo ./install
```
Install the Debian Store extension to generate the Simlink quickly 

## SHELL: ZSH

Making the terminal a bit more useful using the ZSH shell

On Ubuntu:
```
sudo apt install zsh
chsh -s $(which zsh)  #Set zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" #install Oh my zsh
sudo apt-get install fonts-powerline #install font
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k #Install powerline9k theme
. ~/.zshrc #Reload source
```

https://github.com/sindresorhus/pure

https://github.com/agnoster/agnoster-zsh-theme

https://github.com/powerline/fonts


### Powerline -> Powerlevel9k

You then need to select this theme in your ~/.zshrc:

ZSH_THEME="powerlevel9k/powerlevel9k"

Please note if you plan to set a POWERLEVEL9K_MODE to use a specific font, as described in this section of the Wiki, you must set the MODE before OMZ is loaded (look for source $ZSH/oh-my-zsh.sh in your ~/.zshrc).

# PERZONALIZATION

# Fonts

A good system needs a selection of strong fonts, specially on a lowres screen like mine. 

Installing fonts in Linux is incredibly simple, just move all .ttf/.tta files to the .fonts dir in the user home and run:

```
sudo fc-cache
```
Some fonts have their own packages though

| Sans-Serif |
|:---|
| Roboto ```sudo apt-get install fonts-roboto```|
| Microsoft Fonts ``` sudo apt-get install ttf-mscorefonts-installer ``` A must have when using LibreOffice|
| [Fantasque Sans](https://github.com/belluzj/fantasque-sans)  |
| [Inconsolata](https://github.com/googlefonts/Inconsolata)|

| Serif |
|:---|
|[Dejavu](https://dejavu-fonts.github.io/) |
|Noto Fonts ```sudo apt-get install fonts-noto```|

| Terminal (Monospace) |
|:---|
|~~[Iosevka](https://typeof.net/Iosevka/)~~ (Skip in favour of nerdfonts version) A personal favorite of mine, set "Iosevka Term Regular". |
|[Bitmap fonts](https://github.com/Tecate/bitmap-fonts) A collection of bitmap fonts|
|[Nerdfonts](https://www.nerdfonts.com/) Patched fonts for terminal, has many common ones|
|[Powerline Fonts](https://github.com/powerline/fonts) Specially made for usage with Powerline|
|[Awesome Font](https://fontawesome.com/) Used widely by many applications|

| Japanese |
|:---|
| [M+ Fonts](https://mplus-fonts.osdn.jp/about-en.html) ```sudo apt-get install fonts-mplus```|
| [Adobe Source Hans](https://github.com/adobe-fonts/source-han-sans/tree/release)|


# i3-gaps

Some [nice](https://geekoverdose.wordpress.com/2017/02/05/i3-window-manager-setup-and-configuration/) 
[guides](https://github.com/addy-dclxvi/i3-starterpack)
on how to set up everything from people smarter than me

And how to use i3 on [multiple monitors](https://fedoramagazine.org/using-i3-with-multiple-monitors/)
```
xrandr # Get screen info
# HDM1 -> Screen output
# LVDS1 -> My screen
xrandr --output HDM1 --auto --right-of LVDS1 # On my system

```


[link](https://github.com/Airblader/i3)
[install](https://github.com/Airblader/i3/wiki/Installation)
Add external repo and install

```
#Ubuntu
sudo add-apt-repository ppa:kgilmer/speed-ricer
sudo apt-get update
sudo apt install i3-gaps-wm
```

```
sudo apt-get install i3-wm dunst i3lock i3status suckless-tools rofi
```

DPI shenanigans
http://www.idc-online.com/technical_references/pdfs/electronic_engineering/Change_DPI_in_Ubuntu.pdf
112 DPI 1.16666


## i3-bar
//TODO transition to i3 blocks

Config on normal file
i3status

```
sudo apt-get install i3-wm dunst i3lock i3status suckless-tools
```


## Terminal: St

Very interesting approach to personalization. The config file IS the source code! Cahnge and straight up recompile the program.

```
#Download, unzip, and run to install
#sudo make install
sudo apt install stterm

sudo update-alternatives --config x-terminal-emulator

```

## dmenu -> rofi
Copy config file



## Drive

https://github.com/odeke-em/drive

Install
https://github.com/odeke-em/drive/blob/master/platform_packages.md

```
sudo apt-get install software-properties-common dirmngr

sudo apt-add-repository 'deb http://shaggytwodope.github.io/repo ./'
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 7086E9CC7EC3233B
sudo apt-key update
sudo apt-get update
sudo apt-get install drive
```

Set up credentials
```
drive init ~/gdrive
cd ~/gdrive
```

Ok, workflow as follows!

cd to folder:
```bash
cd /media/veracrypt3/Drive/
```

I created something on my machine, want to upload it
```bash
drive push -no-clobber -ignore name clashes path/to/file/or/folder
```

I uploaded/created something on drive, want to donwload it
```bash
drive pull -no-clobber -ignore name clashes path/to/file/or/folder
```

I changed stuff on one end, want to update the other
```bash
drive pull/push -ignore-name-clashes path
```


You can export docs as txt!
```
drive pull -export pdf,rtf,docx,txt
```

List remote...
```
drive list 
```

```
drive clashes --fix
```

## Google Chrome

Makes syncing with phone so much easier. Should I switch to Chromium or Firefox?

[Chrome](https://www.google.com/chrome/)

## Spotify

For better hack-abiliy, add Spotify repository instead of installing through the store. 

[Spotify Download](https://www.spotify.com/es/download/linux/)

```
#Add repo
curl -sS https://download.spotify.com/debian/pubkey.gpg | sudo apt-key add - 
echo "deb http://repository.spotify.com stable non-free" | sudo tee /etc/apt/sources.list.d/spotify.list
#Install
sudo apt-get update && sudo apt-get install spotify-client
```

Avoid Spotify from being muted during calls... for people like me who use spotify during Skype calls

```
#https://unix.stackexchange.com/questions/159104/how-to-stop-gnome-from-muting-my-music
#Simply comment out the line with load-module module-role-cork from the file /etc/pulse/default.pa
```




## Gnome tweak tool

How else are we applying those custom fonts and icon packs?

```
sudo apt install gnome-tweak-tool
```

## Papirus icons


[Papirus](https://github.com/PapirusDevelopmentTeam/papirus-icon-theme/)

```
sudo add-apt-repository ppa:papirus/papirus
sudo apt-get update
sudo apt-get install papirus-icon-theme
```



# Going the extra mile



## Pip3

[Pip3](https://pip.pypa.io/en/stable/)

Pip3 python package installer. Dont use `sudo` ...

```
# Ubuntu
sudo apt install python3-pip
```


## Pywal


```
sudo apt install imagemagick
```
System-wide install

```
#Not super recommended to use sudo with pip3, but pywal is the basis of my customization style
sudo pip3 install pywal
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

### Pywalfox

```
pip install pywalfox
pywalfox install
```



## Mutt
ncmpcpp
powerline

## gcalcli

```
sudo apt-get install gcalcli
```
## ncmpcpp
https://addy-dclxvi.github.io/post/configuring-ncmpcpp/

sudo apt-get install mpd mpc ncmpcpp


# Software
Inkscape
Krita
Blender
Gimp
Chrome
Spotify

# Wine

https://wiki.winehq.org/Debian
https://wiki.debian.org/Wine
https://www.gloriouseggroll.tv/how-to-get-out-of-wine-dependency-hell/

```
sudo dpkg --add-architecture i386 && sudo apt update


sudo dpkg --add-architecture i386
wget -nc https://dl.winehq.org/wine-builds/winehq.key
sudo apt-key add winehq.key

# add to 
# /etc/apt/sources.list:	deb https://dl.winehq.org/wine-builds/debian/ buster main

sudo apt install --install-recommends winehq-stable
sudo dpkg --add-architecture i386 && sudo apt update
sudo apt install \
      wine \
      wine32 \
      wine64 \
      libwine \
      libwine:i386 \
      fonts-wine

# NOT SURE
sudo apt-get install mono-complete
```


# Matlab

```
#To install, extract and:
xhost +SI:localuser:root
sudo bash install
#use default paths

#If we need reactivation
/usr/local/MATLAB/R20XXx/bin/activate_matlab.sh
```




## Steam

```
# add to /etc/apt/sources.list
# deb http://deb.debian.org/debian/ buster main contrib non-free
# 
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install steam-installer

# sudo apt install libgtk2.0-0:i386 vulcan-tools vulcan-utils

sudo apt-get install libnss-resolve:i386


# I dont have a grpahics card :D

sudo apt install libgl1:i386 mesa-vulkan-drivers:i386 mesa-vulkan-drivers
sudo apt-get install winehq-staging
sudo apt-get install winetricks
```


To uninstall:

```
sudo apt-get purge steam steam-launcher
```

# Arduino

Download and...

```
tar xvf FILENAME
cd PACKAGENAME
sudo ./install.sh


#ITS NOT WORKING
sudo chmod a+rw /dev/ttyUSB0]

```

# Skype

```
wget https://go.skype.com/skypeforlinux-64.deb
sudo apt install ./skypeforlinux-64.deb
```

# Virtual Box?

No longer on Debian
https://packages.debian.org/buster/virt-manager

virt-manager

```

wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
echo "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian bionic contrib" | sudo tee /etc/apt/sources.list.d/virtualbox.list
sudo apt install virtualbox-6.0
```


### Succesfull brightness fix 

https://askubuntu.com/questions/715306/xbacklight-no-outputs-have-backlight-property-no-sys-class-backlight-folder




# Cheatsheet

Installing deb packages

```
#To install .deb package
sudo dpkg -i DEB_PACKAGE
sudo dpkg –-remove skypeforlinux
```






# Windows

[Nanite installer with my base apps](https://ninite.com/7zip-blender-firefox-foxit-gimp-googlebackupandsync-inkscape-libreoffice-notepadplusplus-skype-spotify-steam-vlc-vscode/)  
7zip Blender Firefox Foxit Reader Gimp GDrive Sync Inkscape Libreoffice Notepad++ Skype Spotify Steam VLC Player VS Code,

And not on Ninite

- [VERACRYPT](https://www.veracrypt.fr/en/Home.html)
- [MATLAB](https://matlabacademy.mathworks.com/)
- [FUSION360](https://www.autodesk.com/)
- [OFFICE](https://www.office.com/) (if license)
- [GITHUB DESKTOP](https://desktop.github.com/)
- LENOVO THING
