# How to install nvidia driver on Linux system
Source https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#driver-installation
&
https://docs.nvidia.com/datacenter/tesla/tesla-installation-notes/index.html

Download https://www.nvidia.com/Download/index.aspx

## Ubuntu 18.04
### Method 1 (easiest)
Select available driver from Software & Updates application

![Softwares & Updates](../images/softwares_n_updates.png)

### Method 2 (using pakage manager)
Step 1: List out all available nvidia drivers
```sh
sudo apt update
sudo apt list nvidia-driver-*
```

Step 2: Choose one of the list of available drivers & install
![Driver list](../images/cmdl_nvidia_driver.png)
```sh
sudo apt install nvidia-driver-<branch>
```

### Method 3 (manually)
#### Pre-installation Actions
Step 1: Disable the Nouveau drivers

_ Create a file at /etc/modprobe.d/blacklist-nouveau.conf with the following contents:
```cfg
blacklist nouveau
options nouveau modeset=0
```
Quick cmd
```sh
sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
```
_ Regenerate the kernel initramfs:
```sh
sudo update-initramfs -u
```

Step 2: Download & Calculate the MD5 checksum of the downloaded file

```sh
md5sum <file>
```

Step 3: Install kernel headers & necessary packages

```sh
sudo apt install linux-headers-$(uname -r) libglvnd-dev pkg-config dkms
```

Step 4: Uninstall old installation

Depend on the way the previous nvidia driver was installed, unistall it using one of below methods:

_ For installing using pkg manager
```sh
sudo apt remove nvidia-driver-<branch>
```

_ For installing using *.run file
```sh
sudo ./<driver_file>.run --uninstall
```

#### Installation steps
Step 1: Reboot and login to virtual console (tty) by pressing `ctrl + alt + F2/.../F6`

Step 2: Go to the directory which contains downloaded drivers & run the *.run file (with options if necessary. Ex: `--dkms`)
```sh
sudo ./<driver_file>.run --dkms
```

**Note:** See [here](https://github.com/nhphuong91/Linux/blob/master/nvidiaDriverInstallation/OptionToInstallNvidiaDriver.txt) for list of avalable options

## CentOS 7
### Pre-installation Actions
Step 1: Install necessary packages
```sh
sudo dnf install -y tar bzip2 make automake gcc gcc-c++ pciutils elfutils-libelf-devel libglvnd-devel iptables firewalld vim bind-utils wget
```

Step 2: Install kernel headers
```sh
sudo yum install kernel-devel-$(uname -r) kernel-headers-$(uname -r)
```

Step 3: Uninstall old installation

Depend on the way the previous nvidia driver was installed, unistall it using one of below methods:

_ For installing using pkg manager
```sh
sudo yum remove nvidia-driver-latest-dkms
```

_ For installing using *.run file
```sh
sudo ./<driver_file>.run --uninstall
```

### Method 1 (using pakage manager)
Step 1: Satisfy the external dependency on EPEL for DKMS
```sh
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

Step 2: Install the CUDA repository public GPG key
```sh
distribution=rhel7
ARCH=$( /bin/arch )
sudo yum-config-manager --add-repo http://developer.download.nvidia.com/compute/cuda/repos/$distribution/${ARCH}/cuda-$distribution.repo
```

Step 3: Update the repository cache and install the driver using the nvidia-driver-latest-dkms meta-package
```sh
sudo yum clean expire-cache
sudo yum install -y nvidia-driver-latest-dkms
```

### Method 2 (manually)
Step 1: Download & Calculate the MD5 checksum of the downloaded file
```sh
md5sum <driver_file>
```

Step 2: Go to the directory which contains downloaded drivers & run the *.run file (with options if necessary. Ex: `--dkms`)
```sh
sudo ./<driver_file>.run
```