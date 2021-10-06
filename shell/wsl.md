# windows subsystem linux

_[official full instructions](https://docs.microsoft.com/en-us/windows/wsl/install)_

1. install [Windows Terminal](https://docs.microsoft.com/en-us/windows/terminal/get-started)

2. install wsl
_`windows: powershell`_  
```shell
# Installs WSL2, and Ubuntu distro
wsl --install
```

3. install [wslu](https://github.com/wslutilities/wslu) inside the wsl vm for neat utils

## developing code

* running and debugging projects is straightforward when done inside the wsl vm
* editing code _can_ be done outside the wsl vm, but editors won't be able to use project-specific tools like eslint etc, nor system-wide tools like git

### webstorm

webstorm can run inside the wsl vm, as long as there is an X11 server to display too. pretty cool!

follow these [instructions from Alex-D](https://github.com/Alex-D/dotfiles#install-intellij-idea)

## docker

Downloading install [Docker Desktop for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows/), ensuring the "WSL2 Backend" option is ticked during setup.

* If the Ubuntu distro is having trouble connecting to the Docker engine, ensure it is turned on in Docker Desktop `Settings > Resources > WSL Integration`
* Restart the Ubuntu distro (`wsl -t Ubuntu`), and potentially restart the Windows PC

## wireguard

issues:
* by default, the linux distro will get fed it's `/etc/resolv.conf` nameserver automatically; it will point to a virtual Ethernet network adapter
* that is great for almost all situations, but by default it does not route networking through the an active Wireguard tunnel on the Windows host

fixes:
* use wireguard from inside the wsl vm instead
  * run `sudo apt-get install wireguard-tools openresolv`  
    _(`openresolv` **must** be used instead of the default `resolvconf` package, of `wg-quick` will silently fail updating the dns)_
  * restart wsl, then from inside ubuntu test by running `wg-quick up <conf-name>` then `nslookup google.com`; the nameserver used should match whats in your wireguard conf