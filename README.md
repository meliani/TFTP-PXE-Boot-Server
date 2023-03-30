# TFTP-PXE-Boot-Server

This project contains the basic files and folder setup needed for a TFTP PXELINUX server.

Network (PXE) boot supports the following live CD or installation distros.
* CentOS 6.x
* CentOS 7.0
* CloneZilla Live
* Fedora 24
* GParted Live
* Ubuntu 16.04 (Xenial)

### Usage
1. Set up a TFTP server
  * You could use http://ww2.unime.it/flr/tftpserver/ for MacOS
  * Follow [these](http://www.pyrosoft.co.uk/blog/2013/01/13/setting-up-a-pxe-boot-server-on-synology-dsm-4-2-beta/) and [these](https://kb.synology.com/en-us/DSM/tutorial/How_to_implement_PXE_with_Synology_NAS) instructions on Synology NAS
1. Check out this project code on the TFTP server
  `git clone --depth 1 git@github.com:paulmaunders/TFTP-PXE-Boot-Server.git .`
1. Ensure TFTP server root points at this project folder
  * Debug TFTP server as per instructions [below](#Debug_TFTP_server)
1. Set up your DHCP to use TFTP server
  * Use DHCP option 66 "next-server" if located on a different IP to the DHCP server
  * DHCP should offer the PXELINUX.0 as the boot filename (DHCP option 67)
  * Follow [these](https://community.synology.com/enu/forum/2/post/124897) instructions on Synology router
1. Optionally edit the _pxelinux.cfg/default_ file to add in your PXE boot options
1. Download and configure each bootstrap program you require as per instructions. Bootstraps are not committed to this repository due to their size.
  * Alpine Linux (TODO)
  * CentOS (TODO)
  * CloneZilla Live (TODO)
  * Fedora (TODO)
  * GParted Live (TODO)
  * Ubuntu Desktop (TODO)
  * Ubuntu Minimal (TODO)
  * Ubuntu Server (TODO)

### Debug TFTP server
_tftp_ is the user interface to the Internet TFTP (Trivial File Transfer Protocol), which allows users to transfer files to and from a remote machine. The remote host may be specified on the command line, in which case tftp uses host as the default host for future transfers.
1. Install TFTP client
  ```
   user:~$ sudo apt update
   user:~$ sudo apt install tftp
  ```
1. Test connection and file download
  ```
  user:~$ tftp 192.168.0.123
  tftp> verbose
  Verbose mode on.
  tftp> get pxelinux.0
  getting from 192.168.0.123:pxelinux.0 to pxelinux.0 [netascii]
  Received 26579 bytes in 0.4 seconds [531580 bits/sec]
  tftp> quit
  ```

---
### Further documentation
The Syslinux Project https://www.syslinux.org/
PXELINUX is a Syslinux derivative for booting from a network server https://wiki.syslinux.org/wiki/index.php?title=PXELINUX

* Configuration introduction can be found [here](https://wiki.syslinux.org/wiki/index.php?title=Config)
* Advanced menu system configuration documentation can be found [here](https://wiki.syslinux.org/wiki/index.php?title=Menu)

##### How to upgrade Syslinux
1. Clone repository
  `git clone --depth 1 https://github.com/paulmaunders/TFTP-PXE-Boot-Server .`
1. Download new version from https://wiki.syslinux.org/wiki/index.php?title=Download
  `wget https://mirrors.edge.kernel.org/pub/linux/utils/boot/syslinux/Testing/6.04/syslinux-6.04-pre1.zip`
1. Extract modules and dependencies according to https://wiki.syslinux.org/wiki/index.php?title=Library_modules
  > * bios/com32/elflink/ldlinux.c32
  * bios/com32/libutil/libutil.c32
  * bios/com32/menu/menu.c32
  * bios/com32/modules/linux.c32
  * bios/core/pxelinux.0
  * bios/memdisk/memdisk
1. DO NOT use Syslinux versions 5.10 to 6.02 due to known bug which crashes VM
  * https://www.virtualbox.org/ticket/13048
  * https://bugzilla.syslinux.org/show_bug.cgi?id=54
