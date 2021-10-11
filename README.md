# PYNQ on Zynq Board (zc702)


### Directly use the image for running pynq on zc702:

- Get the Image file from [Here](https://drive.google.com/file/d/1yqIqKBsLTee0bAPET0PcrAWoWAk6VIH9/view?usp=sharing)
- Untar `ZC702-2.6.0.tar.xz`
- flash the image to sd card using [balena etcher](https://www.balena.io/etcher/) or dd <br>
`sudo dd bs=1M if=<image-name>.img of=/dev/<sdcard>`
- Insert the SDCARD into the board
- **ON ZC702 keep SW16 boot config switch position: 00110**
- You can login to the board accessing the serial port

### Issues:

- [x] Solve the jupyter access issue.
    - There are two ways for network connection: 
        1) Connect board to common router
        2) Connect board ethernet port directly to PC
    - More Info [Here](https://pynq.readthedocs.io/en/v2.0/getting_started.html#connect-to-a-network-router)
    - I'm using the first option:
        1) First check whether the network port running on board using `ping 8.8.8.8`
        2) Then run `ifconfig` and find out the address of eth0 inet.
        3) On host computer `http://<etho0-address>:9090`
        4) Ensure port 9090 is not blocked by your router/isp.
        5) Login with password `xilinx`
- [ ] PYNQ-DPU port on zc702
    - Xilinx has a DPU library, it is directly supported to pynq boards via [pynq-dpu](https://github.com/Xilinx/DPU-PYNQ) repo and vitis AI.
    - We will need to port it for ZC702.
    - Reference TRD: [Zynq-7000-DPU-TRD](https://github.com/sumilao/Zynq-7000-DPU-TRD)
- [ ] Figure out the way to include FPGA bit file on the board

### Build the image with customized conf file:

- create a `test_repo/ZC702` folder inside `<pynq>/sdbuild`
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