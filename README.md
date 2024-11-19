# pynq_zcu102
# STEPS to setup PYNQ on ZCU102 board:

# Note: This git clone should not be on NFS.

# Dependencies:
# - Vivado 2022.1
# - Petalinux 2022.1

# Clone the PYNQ repository
git clone https://github.com/Xilinx/PYNQ.git
cd PYNQ

# Setup the host environment
cd sdbuild/scripts
./setup_host.sh

# Source the required settings
source /opt/tools/petalinux/settings.sh
source /tools/xilinx/Vivado/2022.1/settings64.sh

# Download the ZCU102 BSP file
# Download from: https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-design-tools/archive.html

# Modify the board setup
cd ../../
cp -rf ./boards/ZCU104 ./boards/ZCU102
rm -rf ./boards/ZCU102/petalinux_bsp/
mv ~/Downloads/xilinx-zcu102-v2022.1-final.bsp ./boards/ZCU102/
mv ./boards/ZCU104/ZCU104.spec ./boards/ZCU102/ZCU102.spec

# Edit the ZCU102.spec file
vim ./boards/ZCU102/ZCU102.spec

# Modify the content of ZCU102.spec as follows:
# ARCH_ZCU102 := aarch64
# BSP_ZCU102 := xilinx-zcu102-v2022.1-final.bsp
# FPGA_MANAGER_ZCU102 := 1
# STAGE4_PACKAGES_ZCU102 := xrt pynq ethernet sensorconf boot_leds pynq_peripherals

# Download the ROOTFS and Pynq prebuilt source distribution
# Download from: https://www.pynq.io/boards.html

# Move the downloaded files to the required directories
mv /path/to/downloaded/pynq_rootfs.aarch64.tar.gz PYNQ/sdbuild/prebuilt/pynq_rootfs.aarch64.tar.gz
mv /path/to/downloaded/pynq_sdist.tar.gz PYNQ/sdbuild/prebuilt/pynq_sdist.tar.gz

# Make sure the files are renamed as:
# - pynq_rootfs.aarch64.tar.gz
# - pynq_sdist.tar.gz

# Build the PYNQ image
cd PYNQ/sdbuild
make BOARDS=ZCU102

# The generated PYNQ image will be available in the following path:
# sdbuild/output/ZCU102.img

# Write the image to the SD card
# Check where the SD card is mounted
df -T

# Use the following command to write the image onto the SD card (replace /dev/sdb with your SD card's device name):
sudo dd bs=1M if=sdbuild/output/ZCU102.img of=/dev/sdb

# Note: Ensure /dev/sdb is the correct device name without any partition number.
