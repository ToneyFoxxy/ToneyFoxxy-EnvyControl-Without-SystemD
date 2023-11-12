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

I have modified EnvyControl to work across all popular init systems, however automatic display manager detection is disabled due to there being no good way to achieve it without SystemD. In practical terms, the only change is that you need to specify a display manager.

Please refer to https://github.com/bayasdev/envycontrol for more information about EnvyControl

### Getting EnvyControl Without SystemD

#### Artix - WIP

1. Install `envycontrol-openrc` or `envycontrol-dinit` or `envycontrol-runit` or `envycontrol-s6` from the AUR
  
3. Run `sudo envycontrol -s <MODE> --dm <DISPLAY MANAGER>` to switch graphics modes

#### Manual
1. Install `nvidia-utils-openrc` or `nvidia-utils-dinit` or `nvidia-utils` and `nvidia` from the World repo

2. Install https://github.com/bayasdev/envycontrol

3. If you are using Runit or S6, you will need to install the nvidia-persistenced daemon
   - Runit: `sudo curl https://raw.githubusercontent.com/NVIDIA/nvidia-persistenced/main/init/sysv/nvidia-persistenced.template > /etc/runit/sv/nvidia-persistenced.txt && chmod +x /etc/runit/sv/nvidia-persistenced.txt`

   - S6: `sudo curl https://raw.githubusercontent.com/NVIDIA/nvidia-persistenced/main/init/sysv/nvidia-persistenced.template > /etc/s6/sv/nvidia-persistenced.txt && chmod +x /etc/s6/sv/nvidia-persistenced.txt`

4. Install my version of envycontrol.py:
   - OpenRC - `sudo rm /usr/lib/python3.11/site-packages/envycontrol.py && curl https://raw.githubusercontent.com/ToneyFoxxy/ToneyFoxxy-EnvyControl-Without-SystemD/main/OpenRC/envycontrol.py > /usr/lib/python3.11/site-packages/envycontrol.py`
     
   - Dinit - `sudo rm /usr/lib/python3.11/site-packages/envycontrol.py && curl https://raw.githubusercontent.com/ToneyFoxxy/ToneyFoxxy-EnvyControl-Without-SystemD/main/Dinit/envycontrol.py > /usr/lib/python3.11/site-packages/envycontrol.py`
     
   - Runit - `sudo rm /usr/lib/python3.11/site-packages/envycontrol.py && curl https://raw.githubusercontent.com/ToneyFoxxy/ToneyFoxxy-EnvyControl-Without-SystemD/main/Runit/envycontrol.py > /usr/lib/python3.11/site-packages/envycontrol.py`
     
   - S6 - `sudo rm /usr/lib/python3.11/site-packages/envycontrol.py && curl [URL](https://raw.githubusercontent.com/ToneyFoxxy/ToneyFoxxy-EnvyControl-Without-SystemD/main/S6/envycontrol.py) > /usr/lib/python3.11/site-packages/envycontrol.py`

5. Run `sudo envycontrol -s <MODE> --dm <DISPLAY MANAGER>` to switch graphics modes

6. Reboot

## Frequently Asked Questions (FAQ)

[Read here](https://github.com/bayasdev/envycontrol/wiki/Frequently-Asked-Questions)

## I have a problem

Open an issue and **don't forget to complete all the requested fields!**

## Buy the original author a coffee

https://www.paypal.com/paypalme/bayasdev
