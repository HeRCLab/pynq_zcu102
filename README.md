# PYNQ ZCU102 Setup:

## STEPS to setup pynq on zcu102 board:

Note: this git clone should not be on nfs.

Dependencies: Vivado 2022.1 and petalinux 2022.1

1. git clone [https://github.com/Xilinx/PYNQ.git](https://github.com/Xilinx/PYNQ.git)
2. cd PYNQ
3. cd sdbuild/scripts
4. ./setup\_host.sh
5. source /opt/tools/petalinux/settings.sh
6. source /tools/xilinx/Vivado/2022.1/settings64.sh
7. download zcu102 bsp file from [https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-design-tools/archive.html](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-design-tools/archive.html)
8. cd ../../
9. cp -rf ./boards/ZCU104 ./boards/ZCU102
10. &#x20;rm -rf ./boards/ZCU102/petalinux\_bsp/
11. mv \~/Downloads/xilinx-zcu102-v2022.1-final.bsp ./boards/ZCU102/
12. mv ./boards/ZCU104/ZCU104.spec ./boards/ZCU102/ZCU102.spec
13. vim ./boards/ZCU102/ZCU102.spec
14. modify the content as follows:\
    ARCH\_ZCU102 := aarch64 BSP\_ZCU102 := xilinx-zcu102-v2022.1-final.bsp

    FPGA\_MANAGER\_ZCU104 := 1

    STAGE4\_PACKAGES\_ZCU102 := xrt pynq ethernet sensorconf boot\_leds pynq\_peripherals\

15. &#x20;Download the ROOTFS and Pynq prebuilt source distribution from [https://www.pynq.io/boards.html](https://www.pynq.io/boards.html)\
    &#x20;move this file to PYNQ/sdbuild/prebuilt/pynq\_rootfs.aarch64.tar.gz\
    and the sdist file to PYNQ/sdbuild/prebuilt/pynq\_sdist.tar.gz\
    make sure the names of the files are rename to pynq\_rootfs.aarch64.tar.gz and pynq\_sdist.tar.gz
16. cd PYNQ/sdbuild&#x20;
17. make BOARDS=ZCU102
18. This will give an pynq image named ZCU102.img in the sdbuild/output directory.
19. Now to write this image onto the sd card:\
    check where the sd card is mounted using:\
    df -T and check the partition name&#x20;
20. Use this command to write onto the sd card the /dev/sdb (should be without number):\
    sudo dd bs=1M if=ZCU102.img of=/dev/sdb







