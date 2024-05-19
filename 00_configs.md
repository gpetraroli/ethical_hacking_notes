# Configs

## WiFi adapter RTL8812AU based products drivers
(Alfa Network AWUS036AC, AWUS036ACS)

```
sudo apt update
sudo apt install realtek-rtl88xxau-dkms
```

This adapters may not work once the drivers are installed, if this is the case the drivers needs to be compiled directly from the repository:

```
sudo apt update
sudo apt upgrade -y
sudo apt dist-upgrade -y
sudo reboot now
sudo apt update
sudo apt install realtek-rtl88xxau-dkms
sudo apt install dkms
git clone https://github.com/aircrack-ng/rtl8812au
cd rtl8812au/
make
sudo make install
```
> **_NOTE:_**<br>
When running ```make``` you may get an error that looks like this:<br>
> ```
> make ARCH=x86_64 CROSS_COMPILE= -C /lib/modules/6.6.15-amd64/build M=/home/kali/rtl8812au  modules
> make[1]: *** /lib/modules/6.6.15-amd64/build: No such file or directory.  Stop.
> make: *** [Makefile:1730: modules] Error 2
> ```
> This may indicates that the build directory for your kernel modules does not exist. This is typically because the necessary kernel headers and development files are not installed on your system.<br>
> To resolve install the kernel headers for your current kernel version. You can do this using the following command:
> ```
> $ sudo apt install linux-headers-$(uname -r)
> ```