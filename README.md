# PYNQ on Zynq Board (zc702)


### Directly use the image for running pynq on zc702:

- Get the Image file from [Here](https://drive.google.com/file/d/1yqIqKBsLTee0bAPET0PcrAWoWAk6VIH9/view?usp=sharing)
- Untar `ZC702-2.6.0.tar.xz`
- flash the image to sd card using [balena etcher](https://www.balena.io/etcher/) or dd <br>
`sudo dd bs=1M if=<image-name>.img of=/dev/<sdcard>`
- Insert the SDCARD into the board
- **ON ZC702 keep SW16 boot config switch position: 00110**
- You can login to the board accessing the serial port
- [ ] Solve the jupyter access issue.
- [ ] PYNQ-DPU port on zc702

### Build the image with customized conf file:

- create a `test_repo/ZC702` folder inside `<petalinux>/sdbuild`
- The `test_repo/ZC702` should contain the ZC702 BSP file which can be downloaded from the xilinx site.
- source the following things if not done already: 
```
export PATH="/opt/crosstool-ng/bin:/opt/qemu/bin:$PATH"
source <path-to-vivado>/Vivado/2018.3/settings64.sh
source <path-to-sdk>/SDK/2018.3/settings64.sh
source <path-to-petalinux>/petalinux-v2018.3-final/settings.sh
petalinux-util --webtalk off
```

- Create a file called `ZC702.spec` inside `test_repo/ZC702` and edit according to the build need. Following the file that I used for the img present in the repo:

```
ARCH_ZC702 = arm
BSP_ZC702 = <bsp-name>.bsp
STAGE4_PACKAGES_ZC702 = ethernet
```

- To build our more customised SD card image while still using the pre-built we can run
`$ make BOARDDIR=test_repo PREBUILT=<recent-arm-image>.img nocheck_images`