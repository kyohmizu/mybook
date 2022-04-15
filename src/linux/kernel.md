# Kernel

https://github.com/torvalds/linux

https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/about/

### Install Kernel

[How to Install the Latest Linux Kernel on Ubuntu & Linux Mint?](https://linuxhint.com/install-linux-kernel-ubuntu/)

https://askubuntu.com/questions/1389585/how-do-i-install-kernel-5-15-7-on-ubuntu-21-10-impish-indri

https://unix.stackexchange.com/questions/685830/how-to-use-5-16-kernel-with-ubuntu-21-10

https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.17-rc7/

```bash
wget http://mirrors.kernel.org/ubuntu/pool/main/o/openssl/libssl3_3.0.1-0ubuntu1_amd64.deb
sudo dpkg -i libssl3_3.0.1-0ubuntu1_amd64.deb

mkdir work
cd work
wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.17-rc7/amd64/linux-headers-5.17.0-051700rc7-generic_5.17.0-051700rc7.202203062330_amd64.deb
wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.17-rc7/amd64/linux-headers-5.17.0-051700rc7_5.17.0-051700rc7.202203062330_all.deb
wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.17-rc7/amd64/linux-image-unsigned-5.17.0-051700rc7-generic_5.17.0-051700rc7.202203062330_amd64.deb
wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.17-rc7/amd64/linux-modules-5.17.0-051700rc7-generic_5.17.0-051700rc7.202203062330_amd64.deb

sudo dpkg -i *.deb
```

### cgroups 

https://sleeplessbeastie.eu/2021/09/10/how-to-enable-control-group-v2/
