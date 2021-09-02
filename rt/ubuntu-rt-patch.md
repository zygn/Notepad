# Install RT Kernel in Ubuntu 18.04 (Linux Kenel ~ 5.4.0-80-generic)

Check this page: https://stackoverflow.com/questions/51669724/install-rt-linux-patch-for-ubuntu

#### Step 1
작업용 폴더 생성 후 진입 

```
mkdir ~/kernel && cd ~/kernel
```

#### Step 2
리눅스 커널 다운로드(https://www.kernel.org/pub/linux/kernel/)) 및 리얼타임 패치 다운로드(https://www.kernel.org/pub/linux/kernel/projects/rt/) 
(이 문서에서는 5.4.143 버전을 사용하였으며, 파일이름은 다음과 같음 `linux-5.4.143.tar.gz`,`patch-5.4.143-rt63.patch.gz`)

**커널과 패치 버전은 무조건 같아야 함**

or, 다음 명령어를 사용하여 다운로드

```
wget https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/5.4/patch-5.4.143-rt63.patch.gz
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-5.4.143.tar.gz
```

#### Step 3
다운로드한 리눅스 커널을 압축 해제 
```
tar -xzvf linux-5.4.143.tar.gz
```

#### Step 4
압축 해제된 리눅스 커널에 RT 패치
```
cd linux-5.4.143
gzip -cd ../patch-5.4.143-rt63.patch.gz | patch -p1 --verbose
```

#### Step 5
커널 컴파일을 위해 필요한 의존성 패키지 및 라이브러리 설치
```
sudo apt install -y libncurses-dev flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf 
```

#### Step 6 
커널 컴파일 하기 전 빌드 옵션 설정
```
make menuconfig

# Make preemptible kernel setup
General setup ---> [Enter]
Preemption Model (Voluntary Kernel Preemption (Desktop)) [Enter]
Fully Preemptible Kernel (RT) [Enter] #Select

# Select <SAVE> and <EXIT>
# Check .config file is made properly
```

#### Step 7
커널 컴파일 
```
make modules
sudo make modules_install -j20
make
sudo make install -j20
```

#### Step 8
커널 적용 및 업데이트
```
cd /boot
ls
sudo update-grub
```

#### Step 9
재부팅 후 커널 버전 확인
```
reboot

...

uname -r 
# 5.4.143-rt63
```
