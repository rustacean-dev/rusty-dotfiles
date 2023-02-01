# rusty-dotfiles
Linux dotfiles for my Rusty Linux Environment

## Install Endeavour OS
Endeavour OS is an awesome Arch Linux Distro. Curerntly it is my Distro of choice :start_struck:
Download an appropriate ISO File, make a bootable USB Stick and boot into it.

I decided to just do a `clean installation without any window manager`. You can also do that by choosing `the online installation option and not choose any distro`.
Furthermore it should be noted that I use a `GRUB 2.0 bootloader` :warning:

## Install relevant software
My current tech stack involves the following sotware:
* Leftwm - A rust based tiling window manager for X11
* Helix - A rust based and totally awesome terminal editor
* Alacritty _ A rust based GPU powered terminal emulator

```sh
sudo pacman -S lightdm leftwm alacritty feh wmctrl wget
```

From the AUR we also need to install a few things:

```
yay -S helix-git picom-pijulius-git ttf-jetbrains-mono eww
```


## Setup Login Manager
As a login manager I use lightdm with nody-greeter. First we need to enable the lightdm service.

### Enable LightDM Service

```bash
sudo systemctl enable lightdm.service
```


### Setup monitors and lightdm config file
Next adapt the `monitor.sh` file to your liking. Then make it executable with the following command:
```sh
chmod +x ./monitor.sh
```

Copy it over to `/usr/share`. Then we are gonna tell lightdm to load this config. This way the monitor stuff is already set when we login:

```sh
sudo cp ./monitor.sh /usr/share
```

Next edit the `lightdm.conf` file in `/etc/lightdm/lightdm.conf` and add the script to the display setup:
We also set nody-greeter as the session manager. We will install it in a second

```sh
...
greeter-session=nody-greeter
display-setup-script=/usr/share/monitor.sh
...
```
### Install nody-greeter
nody-greeter needs node and npm. The easiest and most flexible way to set this up is via `nvm (node version manager)`.
You can find the link to the Github Repo ![here](https://github.com/nvm-sh/nvm).

Essentially you can just run this command here

```sh
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
```

Next install node version 16 via `nvm install 16` followed by a `nvm use 16`

Then we have all dependencies to install nody-greeter.
The Github Repo for nody-greeter can be found ![here](https://github.com/JezerM/nody-greeter)

:warning:
> As we need to clone a repo I advise you to create a dedicated folder for git repos in your home directory. For me this folder is called `GitRepos`.

To install nody-greeter run the following commands:
```sh
git clone --recursive https://github.com/JezerM/nody-greeter.git
cd nody-greeter
npm install
npm run rebuild
npm run build
sudo node make install
```

Then reboot and you should see a nice login screen.

## Install Gruvbox Boot Loader (Grub 2.0)
TODO

## Copy over config files
As a last step simply copy over all relevant config files that you want. Below is an example which assumes that you have cloned this repo into `GitRepos`

```sh
cp -r ~/GitRepos/rusty-dotfiles/alacritty ~/.config
cp -r ~/GitRepos/rusty-dotfiles/helix ~/.config
cp -r ~/GitRepos/rusty-dotfiles/eww ~/.config
cp -r ~/GitRepos/rusty-dotfiles/leftwm ~/.config
cp -r ~/GitRepos/rusty-dotfiles/helix ~/.config
cp ~/GitRepos/rusty-dotfiles/picom.conf ~/.config
```
