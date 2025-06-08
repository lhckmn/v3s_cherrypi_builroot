# LCTech CherryPi Allwinner V3s Buildroot Config


## Brief description
This is an external configuration for building a minimal Linux for the [LCTech CherryPi](https://linux-sunxi.org/CherryPi_PC_V3S) based on the Allwinner V3s SoC.

It uses a Docker container to provide an environment to build the image. 

So far it boots into the shell and outputs the logs of U-Boot and Linux to a LCD connected via the 40-pin connector.\
The LCD is enabled and initialized in U-Boot and the simple-framebuffer is then handed over to Linux.\
Linux is configured to use the simple-framebuffer with DRM.\
It also connects to a wifi network that can be specified in the file "wpa_supplicant.conf" and you can SSH into it.
The root-password is set to "root".


## Features
The following features of the board are integrated or to be integrated:
- [x] LCD support
- [ ] Wifi (onboard ESP8089) --> Driver of ESP8089 seems to be flaky, discarded for now
- [x] Wifi using MT7601u via USB
- [x] SSH via Dropbear
- [ ] On-board audio
- [ ] Ethernet
- [ ] Buttons
- [ ] Flash (onboard W25Q128)


## Using this Repo
### Setup and starting a build
To use this environment with the external config for Buildroot, clone this repo and open it in VSCode.\
Press F1 and choose "Dev Containers: Reopen in Container". A container based on Debian is set up and Buildroot 2025.02 is cloned into the container.\
Once in the container, choose "File" --> "Open Workspace from File..." and open the file "workspace.code-workspace".\
The mounted local folder "external_config" and the cloned version of Buildroot that lives inside the container are visible inside the file explorer on left.\
Open a console using Strg+Shift+`, choose "buildroot" and execute these commands to start your build:
```console
builduser@debian:~$ make BR2_EXTERNAL=../workspace/external_configs cherrypi_defconfig

builduser@debian:~$ make
```
After the build, you can copy the image from Buildroot's output folder to the workspace's root folder to flash it to a SD-Card (helpful if you are working on a host based on Windows).

### The container's internal folder structure
```
/opt/
├─ work/
│  ├─ workspace/            <-- This folder is the local folder that is mounted inside the container
│  │  ├─ external_configs/
│  ├─ buildroot/
```
- A workdir is setup under /opt/work
- The local workspace folder is mounted under /opt/work/workspace
- Buildroot is cloned right next to it under /opt/work/buildroot


## Credits
The patch for the integration of the LCD into U-Boot is fully based on the great work of mcerveny. I adapted his patch to work with U-Boot 2025.04.\
See his repo: [mcerveny/u-boot/tree/simplefb_v3s_v2](https://github.com/mcerveny/u-boot/tree/simplefb_v3s_v2)

The board and buildroot of fademike also helped me a lot, especially when I tried to get the wifi working.\
See his repos: [fademike/Camera_V3s](https://github.com/fademike/Camera_V3s) and [fademike/buildroot_v3s](https://github.com/fademike/buildroot_v3s)