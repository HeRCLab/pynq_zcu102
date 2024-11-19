# Setup PYNQ on ZCU102 Board

Follow these steps to set up PYNQ on the ZCU102 board:

## Note:
- This git clone should **not** be on NFS.
- **Dependencies**:
  - Vivado 2022.1
  - Petalinux 2022.1

### Steps:

```bash
# Clone the PYNQ repository
git clone https://github.com/Xilinx/PYNQ.git
cd PYNQ

# Setup the host environment
cd sdbuild/scripts
./setup_host.sh

# Source the required settings
source /opt/tools/petalinux/settings.sh
source /tools/xilinx/Vivado/2022.1/settings64.sh

# Modify the board setup
cd ../../
cp -rf ./boards/ZCU104 ./boards/ZCU102
rm -rf ./boards/ZCU102/petalinux_bsp/
mv ~/Downloads/xilinx-zcu102-v2022.1-final.bsp ./boards/ZCU102/
mv ./boards/ZCU104/ZCU104.spec ./boards/ZCU102/ZCU102.spec

# Edit the ZCU102.spec file (manually modify the content as described)
vim ./boards/ZCU102/ZCU102.spec

# Move downloaded ROOTFS and Pynq prebuilt source files
mv /path/to/downloaded/pynq_rootfs.aarch64.tar.gz PYNQ/sdbuild/prebuilt/pynq_rootfs.aarch64.tar.gz
mv /path/to/downloaded/pynq_sdist.tar.gz PYNQ/sdbuild/prebuilt/pynq_sdist.tar.gz

# Build the PYNQ image
cd PYNQ/sdbuild
make BOARDS=ZCU102

# Write the image to the SD card
# Replace /dev/sdb with your SD card's device name
sudo dd bs=1M if=sdbuild/output/ZCU102.img of=/dev/sdb
