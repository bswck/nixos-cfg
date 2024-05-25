# bswck's NixOS configuration
## [`home.nix`](home.nix)
Packages, dotfiles, terminal tools, environment variables and aliases.
Add and remove binaries to your global `$PATH` in this file.

Go to [https://search.nixos.org](https://search.nixos.org/packages) to find the
correct package names, though usually they will be what you expect them to be
in other package managers.

`unstable-packages` is for packages that you want to always keep at the latest
released versions, and `stable-packages` is for packages that you want to track
with the current release of NixOS (currently 23.11).

If you want to update the versions of the available `unstable-packages`, run
`nix flake update` to pull the latest version of the Nixpkgs repository and
then apply the changes.

## [`flake.nix`](flake.nix)
Dependencies.
* `nixpkgs` is the current release of NixOS
* `nixpkgs-unstable` is the current trunk branch of NixOS (ie. all the
  latest packages)
* `home-manager` is used to manage everything related to your home
  directory (dotfiles etc.)
* `nur` is the community-maintained [Nix User
  Repositories](https://nur.nix-community.org/) for packages that may not
  be available in the NixOS repository
* `nixos-wsl` exposes important WSL-specific configuration options
* `nix-index-database` tells you how to install a package when you run a
  command which requires a binary not in the `$PATH`
## [`wsl.nix`](wsl.nix)
VM configuration:
* hostname
* default shell
* user groups are set here
* WSL configuration options
* NixOS options

# Installation
* Get the [latest NixOS-WSL installer](https://github.com/nix-community/NixOS-WSL)
* Install it (tweak the command to your desired paths)
```powershell
wsl --import nixos $env:USERPROFILE\nix\ nixos-wsl.tar.gz
```

* Enter the distro
```powershell
wsl -d nixos
```

* Set up a channel
```bash
sudo nix-channel --update
```

* Get a copy of this repo (you'll probably want to fork it eventually)
```bash
nix-shell -p git
git clone https://github.com/bswck/nixos-configuration /tmp/configuration
cd !$
```

* Apply the configuration
```bash
sudo nixos-rebuild switch --flake .
```

* Restart and reconnect to the current WSL shell
```bash
wsl -t nixos
wsl -d nixos
```

* `cd ~` and then `pwd` should now show `/home/bswck`
* Move the configuration to your new home directory 
```bash
mv /tmp/configuration ~/configuration
```
* Apply the configuration
```bash
sudo nixos-rebuild switch --flake !$
```

> [!Note]
> If developing in Rust, you'll still be managing your toolchains and components like `rust-analyzer` with `rustup`!

# Notes
* The default editor is `lvim`
* `win32yank` is used to ensure perfect bi-directional copying and pasting to
  and from Windows GUI applications and LunarVim running in WSL
* The default shell is `zsh`
* Native `docker` (ie. Linux, not Windows) is enabled by default
* The prompt is [Starship](https://starship.rs/)
* [`fzf`](https://github.com/junegunn/fzf),
  [`lsd`](https://github.com/lsd-rs/lsd),
  [`zoxide`](https://github.com/ajeetdsouza/zoxide), and
  [`broot`](https://github.com/Canop/broot) are integrated into `zsh` by
  default
    * These can all be disabled easily by setting `enable = false` in
      [home.nix](home.nix), or just removing the lines all together
* [`direnv`](https://github.com/direnv/direnv) is integrated into `zsh` by
  default
* `git` config is generated in [home.nix](home.nix) with options provided to
  enable private HTTPS clones with secret tokens
* `zsh` config is generated in [home.nix](home.nix) and includes git aliases,
  useful WSL aliases, and
  [sensible `$WORDCHARS`](https://lgug2z.com/articles/sensible-wordchars-for-most-developers/)

