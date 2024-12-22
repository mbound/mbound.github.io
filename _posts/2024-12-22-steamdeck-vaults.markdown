# Enable crypt backend for Plasma Vaults on Steam Deck

Extracted from:
https://www.secureideas.com/blog/encrypting-data-on-the-steam-deck-with-plasma-vaults 


SteamOS base image doesn’t package any of the encryption backends that Plasma Vaults use (yes it's quite silly).  
To fix this, we will need to install one of the following backends - CryFS, EncFS, gocryptfs.

The post linked focuses on `gocryptfs` which is perfectly fine. 

The `gocryptfs` backend doesn't need to be isntalled in a system folder, but can be installed in a local user binary folder, typically on the Steam Deck it is a good idea to use something like `~/.local/bin`.
This is because anytime there is a SteamOS system update, the root file system is wiped and overwritten with the new system image. However, everything under the `/home` directory is untouched by system updates. By moving the gocryptfs package into `~/.local/bin`, we can persist it across SteamOS system updates.

We use a temporary working directory `tmp` to unpack the `gocryptfs` package:

```
mkdir tmp
sudo pacman --cachedir ./tmp -Sw --noconfirm gocryptfs
mkdir -p ~/.local/bin
tar -xf ./tmp/gocryptfs-*.pkg.tar.zst -C ~/.local/bin --strip-components=2 usr/bin
sudo rm -f ./tmp/gocryptfs*.pkg.*
```

The previous commands extract gocryptfs package to the temporary directory, and then unpack it into the `~/.local/bin` directory.

The above assumes that `~/.local/bin` is already in the user's PATH.  If not, we need to add it. The simplest way to do it is to add the following line to your `.bashrc`

```
export PATH=$HOME/.local/bin:$PATH
```

A probably better way to do it that extends beyond `bash` usage is the following:

```
mkdir -p ~/.config/environment.d
echo 'PATH="$PATH:$HOME/.local/bin"' >> ~/.config/environment.d/envvars.conf
echo 'export PATH="$PATH:$HOME/.local/bin"' >> ~/.bash_profile
```
