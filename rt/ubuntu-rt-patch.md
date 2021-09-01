# Install RT Kernel in Ubuntu 18.04 (Linux Kenel ~ 5.4.0-80-generic)

#### Step 1
Make a working directory

```
mkdir ~/kernel && cd ~/kernel
```

#### Step 2
Download Kernel file(https://www.kernel.org/pub/linux/kernel/), Patch file(https://www.kernel.org/pub/linux/kernel/projects/rt/) those must be same version. this documents accroding by 5.4.143

```
wget https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/5.4/patch-5.4.143-rt63.patch.gz
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-5.4.143.tar.gz
```

#### Step 3
Unzip the kernel
```
tar -xzvf linux-5.4.143.tar.gz
```

#### Step 4
RT patch the kernel
```
cd linux-5.4.143
gzip -cd ../patch-5.4.143-rt63.patch.gz | patch -p1 --verbose
```

#### Step 5
install depedencies (to Kernel compile)
```
sudo apt install -y libncurses-dev flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf 
```

#### Step 6 
check the options
```
make menuconfig
```

#### Step 7
Compile the kernel
```
make modules
sudo make modules_install -j20
make
sudo make install -j20
```
