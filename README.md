# Realtek PCIe GBE NIC Driver

Applicable for RTL8111/8168/8411 PCIe GBE NIC.

Using the official driver will resolve sending rate being capped at 4MB/s per tcp connection under tcp-bbr, typically when you're using the default `r8169` nic driver on RTL8168 device.
You need to install the driver only if you did notice the sending rate degradation.
Newer distros might have resolve this issue, but I haven't verified it so far.

1. Install dependences (assume `sudo su`)

    ```bash
    apt update && apt install build-essential libelf-dev linux-headers-$(uname -r)
    ```

    Note: [Download header packages by yourself](http://kernel.ubuntu.com/~kernel-ppa/mainline/) if you are using Ubuntu PPA kernel.

2. Download and install the driver (assume `sudo su`)

    This repo contains a copy of the official driver (8.047.05, tested working as of Linux 5.3):

    ```bash
    git clone https://github.com/airium/Realtek-PCIe-GBE-NIC-Driver.git
    cd Realtek-PCIe-GBE-NIC-Driver/r8168-8.047.05
    sh autorun.sh
    # this script will break the network temporarily
    # on Ubuntu 16/18, your ssh session will be automatically resumed in ~1 minute
    # on Debian 8/9, you might have to reboot manually after ~5 minutes
    ```

    Note: You can also check newer drivers from [Realtek's official site](https://www.realtek.com/en/component/zoo/category/network-interface-controllers-10-100-1000m-gigabit-ethernet-pci-express-software). You probably have to search for new ones if Realtek once again changed their url). [mtorromeo](https://github.com/mtorromeo/r8168) seems to be very happy to do a long-term mirroring.

3. Check if the driver of the network interface in which you are interested has been updated (assume `sudo su`)

    ```bash
    for i in $(ls /sys/class/net); do
        echo $i '====================='
        ethtool -i $i
    done
    ```

    Example of success

    ```text
    enp4s0 =====================
    driver: r8168
    version: 8.047.05-NAPI
    firmware-version:
    expansion-rom-version:
    bus-info: 0000:04:00.0
    supports-statistics: yes
    supports-test: no
    supports-eeprom-access: no
    supports-register-dump: yes
    supports-priv-flags: no
    ```

    Note: You need to re-install the driver after changing kernel.

## Reference

> <https://www.unixblogger.com/how-to-get-your-realtek-rtl8111rtl8168-working-updated-guide/>

Compared with the reference, this guide complements necessary dependencies, and removes the blacklist operation (it seems the Realtek driver will do it for you).
