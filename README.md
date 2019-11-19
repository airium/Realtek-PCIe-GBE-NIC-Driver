# Realtek PCIe GBE NIC Driver

Applicable for RTL8111/8168/8411 PCIe GBE NIC.

Using the official driver will resolve sending rate being capped at 4MB/s per tcp connection under tcp-bbr.

1. Install dependences (assume `sudo su`)

    ```bash
    apt update && apt install build-essential libelf-dev linux-headers-$(uname -r)
    ```

    Note: [Download header packages by yourself](http://kernel.ubuntu.com/~kernel-ppa/mainline/) if you are using Ubuntu PPA kernel.

2. Download and install the driver (assume `sudo su`)

    This repo contains a copy of the official driver (8.047.04, tested working on linux 5.3):

    ```bash
    git clone https://github.com/airium/Realtek-PCIe-GBE-NIC-Driver.git
    cd Realtek-PCIe-GBE-NIC-Driver/r8168-8.047.04
    sh autorun.sh
    # this script will break the network temporarily
    # on Ubuntu 16/18, your ssh session will be automatically resumed in ~1 minute
    # on Debian 8/9, you might have to reboot manually after ~5 minutes
    ```

    Note: You can also check newer drivers from [Realtek's official site](https://www.realtek.com/en/component/zoo/category/network-interface-controllers-10-100-1000m-gigabit-ethernet-pci-express-software) (it seems that Realtek is changing their urls frequently, you probably have to search for new one).

3. Check if the driver of the network interface in which you are interested has been updated (assume `sudo su`)

    ```bash
    for i in $(ls /sys/class/net); do
        echo $i '====================='
        ethtool -i $i
    done
    ```

    Example of success

    ```text
    enp1s0 =====================
    driver: r8168
    version: 8.046.00-NAPI
    firmware-version:
    expansion-rom-version:
    bus-info: 0000:01:00.0
    supports-statistics: yes
    supports-test: no
    supports-eeprom-access: no
    supports-register-dump: yes
    supports-priv-flags: no
    ```

    Note: You need to re-install the driver after changing kernel.

Reference:

> <https://www.unixblogger.com/how-to-get-your-realtek-rtl8111rtl8168-working-updated-guide/>
