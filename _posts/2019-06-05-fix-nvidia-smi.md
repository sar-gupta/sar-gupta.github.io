---
title: "[FIX] NVIDIA Driver Problem "
date: 2019-06-05 00:00:00
permalink: /posts/2019/06/05/fix-nvidia-driver
tags: 
  - nvidia
  - cuda
  - gpu
  - fix
  - driver
---

> This post is for self-reference in case the problem occurs again. 


The problem in question is this:

`NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. Make sure that the latest NVIDIA driver is installed and running.`

<p align="center"><img src="/images/2019-06-05-fix-nvidia-smi/problem.jpeg" alt="image"></p>

I have an NVIDIA GeForce 920M GPU on my laptop, which isn't great for training stuff, but works well for testing, prototyping and running certain github repos out-of-the-box.

I have CUDA-10.1 and cuDNN already installed, and NVIDIA-418 driver was working fine with it. `Something` updated (I don't know what) and the NVIDIA driver stopped working. After hours of searching and trying out different methods, I finally found one that worked for me.

```
$ sudo apt purge nvidia-*
$ sudo ppa-purge ppa:graphics-drivers/ppa
$ sudo apt autoremove
$ sudo apt auto-clean
```

Open /etc/modprobe.d/disable-nouveau.conf to blacklist nouveau.

```
$ sudo vim /etc/modprobe.d/disable-nouveau.conf
```

Paste the following content in it:

```
blacklist nouveau
blacklist vga16fb
blacklist rivafb
blacklist nvidiafb
blacklist rivatv
blacklist amd76_edac
alias nouveau off
alias lbm-nouveau off
options nouveau modeset=0
```

Then re-add the PPA and install the driver.

```
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo ubuntu-drivers autoinstall
```

Reboot.

```
$ sudo reboot
```

After this, the NVIDIA driver was working fine.

<p align="center"><img src="/images/2019-06-05-fix-nvidia-smi/solved.jpeg" alt="image"></p>


### Side notes: 

> To enable GUI from the virtual terminal, run:
```
$ sudo init 5
```

> In case virtual terminal opens up directly on startup, switch to GUI and then run:
```
$ sudo systemctl set-default graphical.target
```






