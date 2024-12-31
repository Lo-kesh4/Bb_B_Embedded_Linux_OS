# Bb_B_Embedded_Linux_OS
Embedded-Linux OS development set-up for "BeagleBone_Black"
> WARNING ---> if we blindly follow the steps provided, the set-up will not work - we need to have a deep understanding and adapt it.
#### Pre-Installation steps
```
df -h
```
see
it would be appropriate to have 150GB to 200GB of free-space

**following are the IMPORTANT PACKAGES to be installed**

let us install a set of packages on a host-development platform 

run and install the following packages
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install aptitude
sudo aptitude install libncurses5-dev:amd64
sudo aptitude install device-tree-compiler lzma lzop libncurses5-dev:amd64 libssl-dev:amd64 minicom 
sudo aptitude install bison
sudo aptitude install flex
sudo apt-get install make
sudo apt-get install gcc
```
_note: The above steps are based on different developers/engineers/vendors, who have installed and tested the development packages/tools_

**Typically**, create a work-space as below:
```
cd  /opt/embedded_ws_Sep2024
```
In addition to the previous packages, we need very, specific packages, for our kernel-development set-up  

you can download required [specific packages](https://drive.google.com/drive/folders/1yFmgkSLnrdrZdk4n1CVy2Q5zhuQqxDCT?usp=drive_link).

Download the specific packages and copy under the `/opt/embedded_ws_Sep2024`
```
tar xf gcc-linaro-6.5.0-2018.12-x86_64_arm-linux-gnueabihf.tar.xz
```
what is the target-machine/cpu/processor ?

arm-32
                
what is the host-machine ?

x86_64

who is the vendor providing the tool-chain ?

Linaro   

```
ls -ld  /opt/embedded_ws_Sep2024/gcc-linaro-6.5.0-2018.12-x86_64_arm-linux-gnueabihf/
```
This PATH environment-variable must be set to include cross tool-chain's binaries' locations - otherwise, cross tool-chain's tools/binaries will not be visible/accessible
```
PATH=/opt/embedded_ws_Sep2024/gcc-linaro-6.5.0-2018.12-x86_64_arm-linux-gnueabihf/bin:$PATH
echo $PATH
```
if we are setting PATH environment variable in a normal-user's shell and use sudo to execute the following command, PATH will not be passed to sudo - due to this, we will get tool-chain errors - so, we need to do the following :
The above PATH should be modified in `/root/.profile` and `/home/<user>/.bashrc` as mentioned below:
```
export PATH=/opt/embedded_ws_Sep2024/gcc-linaro-6.5.0-2018.12-x86_64_arm-linux-gnueabihf/bin:$PATH
```
add the above code into `.profile` and `.bashrc` 

we are using a patched-version of Linux-kernel from a Vendor

let us unpack kernel source-tree from Digi-key 
```
cd /opt/emdedded_ws_Sep2024/
tar zxvf  bb_b_KERNEL.tar.gz
ls -ldi /opt/emdedded_ws_Sep2024/KERNEL
cd KERNEL
```
```
arm-linux-gnueabihf-    ## and use tab key .....it should list cross tool-chain's
```
Is this kernel source-tree same as that of GPOS kernel source-tree ?

The answer is NO - this kernel source-tree is a modified version of a specific SoC/board and provided by community developers - this is very common in embedded OSes - either a community will provide or a vendor will provide, or some combination 

if you are already in kernel source tree`/opt/emdedded_ws_Sep2024/KERNEL`, it is fine -just check that you are in kernel source tree

we need to pass  special variables to Makefile scripts of kbuild

for instance, ARCH=arm will pass a ARCH variable's settings to kbuild's scripts
```
make ARCH=arm mrproper
```
This command is clean-up generic binaries and arch. specific binaries - remove old binaries
we need to provide ARCH=arm, so that arm specific directories and files are affected - otherwise, native-arch/x86_64 specific directories and files will be affected - in addition, architecture-independent directories and files are also affected 

> we must be, in kernel-source tree's top-directory `KERNEL`, as mentioned above - otherwise, there will be errors 
```
cp ../config-bb-b-4.19-eos-drivers .config
```

this config is , for a specific chip-set and board- this kernel-configuration file contains several kernel-configuration parameters, including configuration/selection of embedded-chip/controller/SoC and board many of these selections are different from GPOS-kernel set-up 

In the above step, we are providing an appropriate config file, for kernel-configuration

This will include/enable and exclude/disable appropriate components/sub-systems/modules , as well as set one or more kernel-configuration parameters

once we copy it as a .config, we can build our kernel-image for our target system + other, binaries, like dynamic-modules ...

line no 573
