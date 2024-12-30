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


