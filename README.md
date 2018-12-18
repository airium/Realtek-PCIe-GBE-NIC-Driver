# Realtek PCIe GBE NIC Driver

Applicable for RTL8111/8168/8411 PCIe GBE NIC.

1. Install dependence

    ```bash
    apt update
    apt install build-essential libelf-dev linux-headers-$(uname -r)
    ```

    **if you are using Ubuntu PPA kernel, please download headers source by yourself:* \
    <http://kernel.ubuntu.com/~kernel-ppa/mainline/>

2. Download the driver and install

    This repo contains a copy of the official driver (8.046.00):

    ```bash
    git clone https://github.com/airium/Realtek-PCIe-GBE-NIC-Driver.git
    cd Realtek-PCIe-GBE-NIC-Driver/r8168-8.046.00
    sh autorun.sh
    # this script will break the network temporarily
    # on Ubuntu 16/18, it will automatically restore after ~1 minute
    # on Debian 8/9, you might have to reboot manually
    ```

    You can also checkout newer driver from Realtek official site: \
    <https://www.realtek.com/en/component/zoo/advanced-search/472?Itemid=276>

3. Check if the driver of the network interface you are interested in has been updated

    ```bash
    for i in $(ls /sys/class/net); do
        echo $i '====================='
        ethtool -i $i
    done
    ```

    Example

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

Reference

> <https://www.unixblogger.com/how-to-get-your-realtek-rtl8111rtl8168-working-updated-guide/>
