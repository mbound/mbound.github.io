# How to fix pacman on Steam Deck Desktop mode

Steam OS is an immutable distro, which means every OS update wipes out the user-made changes in the OS root directory. Only `/home` is preserved.
Pacman usually gets the boot, so you may get all sorts of errors, such as:

```
warning: Public keyring not found; have you run 'pacman-key --init'?
downloading required keys...
error: keyring is not writable
error: required key missing from keyring

```
or
```
error: failed to commit transaction (invalid or corrupted package (PGP signature))
```
or
```
error: failed to commit transaction (download library error
```

To fix it, we need to rebuild pacman's keyring and repository list:

First we disable read-only mode:
```
sudo steamos-readonly disable
```
Then we flush the keyrings:
```
sudo rm -rvf /etc/pacman.d/gnupg
sudo pacman-key --init
sudo pacman-key --populate
sudo pacman -S holo-keyring archlinux-keyring
```

And we update the repo mirros:
```
pacman -Sy
```

This should fix Pacman :)
