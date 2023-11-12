<div align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/bayasdev/envycontrol/raw/main/logos/dark.png">
  <img alt="EnvyControl Logo" src="https://github.com/bayasdev/envycontrol/raw/main/logos/light.png" height="100px">
</picture>
<br>
Optimus made easy, without SystemD
</div>
<br>

# EnvyControl

EnvyControl is a CLI tool that provides an easy way to switch between GPU modes on Nvidia Optimus systems (i.e laptops with hybrid Intel + Nvidia or AMD + Nvidia graphics configurations) under Linux. I have modified it to work across all popular init systems, however automatic display manager detection is disabled due to there being no good way to achieve it without SystemD. In practical terms, the only change is that you need to specify a display manager.

### License

EnvyControl is free and open-source software released under the [MIT](https://github.com/bayasdev/envycontrol/blob/main/LICENSE) license.

### Disclaimer

**This software is provided 'as-is' without any express or implied warranty.**

Keep in mind any custom X.org configuration may get deleted or overwritten when switching modes.

### Features

- Written in Python 3+ for portability and compatibility
- Works across all major Linux distros ([tested distros](https://github.com/bayasdev/envycontrol/wiki/Frequently-Asked-Questions#tested-distros))
- Supports GDM, SDDM and LightDM display managers ([manual setup instructions](https://github.com/bayasdev/envycontrol/wiki/Frequently-Asked-Questions#what-to-do-if-my-display-manager-is-not-supported) also available)
- Save battery with integrated graphics mode
- PCI-Express Runtime D3 (RTD3) Power Management support for Turing and later
- Coolbits support for GPU overclocking
- Fix screen tearing with ForceCompositionPipeline

### Graphics modes

#### Integrated

- The integrated Intel or AMD iGPU is used exclusively
- Nvidia dGPU is turned off to reduce power consumption
- External screens cannot be used if the video ports are wired to the dGPU

#### Hybrid

- Enables PRIME render offloading
- RTD3 allows the dGPU to be dynamically turned off when not in use
  - Available choices for the `--rtd3` flag (based on the [official documentation](http://us.download.nvidia.com/XFree86/Linux-x86_64/530.30.02/README/dynamicpowermanagement.html))
    - `0` disabled
    - `1` coarse-grained
    - `2` fine-grained (default value if you don't provide one)
    - `3` fine-grained for Ampere and later
  - Only works in Turing and later
- Performance on external screens might be reduced

#### Nvidia

- The Nvidia dGPU is used exclusively
- Higher graphical performance and higher power consumption
- Recommended when working with external screens
  - If facing screen tearing enable ForceCompositionPipeline with the `--force-comp` flag
- Allows overlocking (not recommended) with the `--coolbits` flag
  - The default value is `28` bits however it can be manually adjusted according to this [guide](https://wiki.archlinux.org/title/NVIDIA/Tips_and_tricks#Overclocking_and_cooling)
- Wayland sessions default to hybrid mode

### Usage

```
usage: envycontrol.py [-h] [-v] [-q] [-s MODE] [--dm DISPLAY_MANAGER] [--force-comp] [--coolbits [VALUE]] [--rtd3 [VALUE]] [--reset-sddm] [--reset] [--verbose]

options:
  -h, --help            show this help message and exit
  -v, --version         Output the current version
  -q, --query           Query the current graphics mode
  -s MODE, --switch MODE
                        Switch the graphics mode. Available choices: integrated, hybrid, nvidia
  --dm DISPLAY_MANAGER  Manually specify your Display Manager for Nvidia mode. Available choices: gdm, gdm3, sddm, lightdm
  --force-comp          Enable ForceCompositionPipeline on Nvidia mode
  --coolbits [VALUE]    Enable Coolbits on Nvidia mode. Default if specified: 28
  --rtd3 [VALUE]        Setup PCI-Express Runtime D3 (RTD3) Power Management on Hybrid mode. Available choices: 0, 1, 2, 3. Default if specified: 2
  --use-nvidia-current  Use nvidia-current instead of nvidia for kernel modules
  --reset-sddm          Restore default Xsetup file
  --reset               Revert changes made by EnvyControl
  --verbose             Enable verbose mode
```

### Example commands

Set graphics mode to nvidia on SDDM display manager

```
sudo envycontrol -s nvidia --dm sddm
```

Set graphics mode to hybrid and enable fine-grained power control:

```
sudo envycontrol -s hybrid --rtd3
```

Set graphics mode to nvidia, enable ForceCompositionPipeline and Coolbits with a value of 24:

```
sudo envycontrol -s nvidia --force-comp --coolbits 24
```

Query the current graphics mode:

```
envycontrol --query
```

Revert all changes made by EnvyControl:

```
sudo envycontrol --reset
```

### Getting EnvyControl Without SystemD

#### Artix - WIP

1. Install `envycontrol-openrc` or `envycontrol-dinit` or `envycontrol-runit` or `envycontrol-s6` from the AUR
2. Run `sudo envycontrol -s <MODE> --dm <DISPLAY MANAGER>` to switch graphics modes

#### Manual

1. Install `nvidia-utils-openrc` or `nvidia-utils-dinit` or `nvidia-utils` and `nvidia`
2. Install https://github.com/bayasdev/envycontrol
3. If you are using Runit or S6, you will need to install the nvidia-persistenced daemon
   - Runit: `sudo curl https://raw.githubusercontent.com/NVIDIA/nvidia-persistenced/main/init/sysv/nvidia-persistenced.template > /etc/runit/sv/nvidia-persistenced.txt && chmod +x /etc/runit/sv/nvidia-persistenced.txt`
   - S6: `sudo curl https://raw.githubusercontent.com/NVIDIA/nvidia-persistenced/main/init/sysv/nvidia-persistenced.template > /etc/s6/sv/nvidia-persistenced.txt && chmod +x /etc/runit/sv/nvidia-persistenced.txt`
4. Install my version of envycontrol.py:
   - OpenRC - `sudo rm /usr/lib/python3.11/site-packages/envycontrol.py && curl https://raw.githubusercontent.com/ToneyFoxxy/ToneyFoxxy-EnvyControl-Without-SystemD/main/OpenRC/envycontrol.py > /usr/lib/python3.11/site-packages/envycontrol.py`
   - Dinit - `sudo rm /usr/lib/python3.11/site-packages/envycontrol.py && curl https://raw.githubusercontent.com/ToneyFoxxy/ToneyFoxxy-EnvyControl-Without-SystemD/main/Dinit/envycontrol.py > /usr/lib/python3.11/site-packages/envycontrol.py`
   - Runit - `sudo rm /usr/lib/python3.11/site-packages/envycontrol.py && curl https://raw.githubusercontent.com/ToneyFoxxy/ToneyFoxxy-EnvyControl-Without-SystemD/main/Runit/envycontrol.py > /usr/lib/python3.11/site-packages/envycontrol.py`
   - S6 - `sudo rm /usr/lib/python3.11/site-packages/envycontrol.py && curl [URL](https://raw.githubusercontent.com/ToneyFoxxy/ToneyFoxxy-EnvyControl-Without-SystemD/main/S6/envycontrol.py) > /usr/lib/python3.11/site-packages/envycontrol.py`
5. Run `sudo envycontrol -s <MODE> --dm <DISPLAY MANAGER>` to switch graphics modes
6. Reboot

### GUIs

#### Gnome Extension

The [GPU profile selector](https://github.com/LorenzoMorelli/GPU_profile_selector) extension provides a simple way to switch between graphics modes in a few clicks, you can get it from [here](https://extensions.gnome.org/extension/5009/gpu-profile-selector/).

**Make sure to have EnvyControl installed globally!**

![gpu profile selector screenshot](https://github.com/LorenzoMorelli/GPU_profile_selector/raw/main/img/extension_screenshot.png)

#### KDE Widget

[Optimus GPU Switcher](https://github.com/enielrodriguez/optimus-gpu-switcher) allows you to change the GPU mode easily, plus its icon is dynamic and serves as an indicator of the current mode.

![Screenshot_20230703_153738](https://github.com/enielrodriguez/optimus-gpu-switcher/assets/31964610/ace0c67e-9428-49fd-895c-48a236727898)

### Tips

#### `nvidia` kernel module is named `nvidia-current` on Debian

If you're running into this situation you can use the `--use-nvidia-current` flag to make EnvyControl use the correct module name.

#### Wayland session is missing on Gnome 43+

GDM now requires `NVreg_PreserveVideoMemoryAllocations` kernel parameter which breaks sleep in nvidia and hybrid mode, as well as rtd3 in hybrid mode, so EnvyControl disables it, if you need a Wayland session follow the instructions below

```
sudo systemctl enable nvidia-{suspend,resume,hibernate}
sudo ln -s /dev/null /etc/udev/rules.d/61-gdm.rules
```

#### The `/usr/share/sddm/scripts/Xsetup` file is missing on my system

If this ever happens please run `sudo envycontrol --reset-sddm`.

## Frequently Asked Questions (FAQ)

[Read here](https://github.com/bayasdev/envycontrol/wiki/Frequently-Asked-Questions)

## I have a problem

Open an issue and **don't forget to complete all the requested fields!**

## Buy the original author a coffee

[PayPal](https://www.paypal.com/paypalme/bayasdev)
