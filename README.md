Arch Linux

Upgrade Kernel on Arch Linux

Arch is a rolling release Linux distro. It means you always get up to date software packages and kernel updates on Arch Linux. 

But that doesn’t mean you can’t manually install an updated version of Kernel on Arch Linux. Of course you can.

In this article, I will show you how to update the kernel of Arch Linux using the package manager. 

I will also show you how to compile the kernel from source and use it on Arch Linux. Let’s get started.

Updating Kernel using Package Manager:

First check the version of kernel you’re currently using with the following command:
```bash
uname -r
```
Now run the following command to perform a system update with pacman:
```bash
yay -S cpio bc rsync gtk2 glib2 libglade
```
Compiling Kernel from Source:

You can also download and compile an updated version of kernel from the official website of Linux kernel at `https://www.kernel.org`

The latest version of Linux kernel as of the time of writing is `6.9.0-rc2`. Find latest kernel copy link use `wget` command.
```bash
wget https://git.kernel.org/torvalds/t/linux-6.9-rc2.tar.gz
```
The Linux kernel archive file should start downloading.

Once the download is complete, navigate to the directory where you downloaded the file. In my case it is the Downloads/ directory in my USER’s home directory.

I listed the directory contents with ls command and as you can see, `linux-6.9-rc2.tar.gz` file is there.

Now extract the archive file with the following command:
```bash
tar xvf linux-6.9-rc2.tar.gz
```
The file should be extracted.

NOTE: To compile a Linux kernel, you need more than `20GB` of free space. You can check how much space you have left with `df -h` command.

Once the file is extracted, a new directory should be created. In my case it is `linux-6.9-rc2/` directory.

Now navigate to the directory with the following command:
```bash
cd linux-6.9-rc2
```
Now copy the configuration file that the current kernel is using to the `linux-6.9-rc2` directory with the following command:
```bash
zcat /proc/config.gz > .config
```
Now run the following command to prepare the configuration file for the new version of kernel.
```bash
make gconfig
```
It should start the following terminal based graphical interface.

From here you can enable or disable specific kernel features. If you don’t know what it is, just leave the defaults.

Once you’re satisfied with the configuration file, go to `Save` option

Go to `File` Go to `Exit`

You should be back to the terminal.

Now run the following command to start the compilation process:
```bash
make -j24
```
The kernel compilation process should start.

It should take a long time for the kernel compilation process to finish.

Now install all the compiled kernel headers with the following command:
```bash
sudo make headers
```
```bash
sudo make headers_install
```
All the kernel headers should be installed.

Now install all the compiled kernel modules with the following command:
```bash
sudo make modules
```
```bash
sudo make modules_install
```
All the kernel modules should be installed.

Now copy the vmlinuz file for your architecture to the /boot directory. For 32-bit operating system, run the following command:
For 32-bit operating system, run the following command:
```bash
sudo cp -v arch/x86/boot/bzImage /boot/vmlinuz-linux-6.9.0-rc2
```
For 64-bit operating system, run the following command:
```bash
sudo cp -v arch/x86_64/boot/bzImage /boot/vmlinuz-linux-6.9.0-rc2
```
The file should be copied.

Now install all the compiled nvidia-dkms with the following command:
```bash
yay -S nvidia-dkms
```
Nvidia-dkms should be installed.

Now generate an initramfs image and save it to /boot directory with the following command:
```bash
sudo mkinitcpio -k 6.9.0-rc2 -g /boot/initramfs-6.9.0-rc2.img
```
The initramfs file should be generated.

Now copy the System.map file to /boot directory with the following command:
```bash
sudo cp -v System.map /boot/System.map-6.9.0-rc2
```
Now make a symbolic link of the System.map-6.9.0-rc2 file to /boot/System.map with the following command:
```bash
sudo ln -sf /boot/System.map-6.9.0-rc2 /boot/System.map
```
Now generate a grub.cfg file with the following command:
```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
A new grub.cfg file should be generated.

Now reboot your computer with the following command:
```bash
sudo reboot
```
When your computer shows the `GRUB` menu, select the `“Advanced options for Arch Linux”` option and press `Enter`.

Then select the menu for your newly installed kernel from the list and press `Enter`.

Once your computer boot, run the following command to check for the kernel version:
```bash
uname -r
```
The kernel should be updated as you can see from the screenshot below.

That’s how you upgrade the kernel of Arch Linux. Thanks for reading this article.


Uninstall instructions removing other kernels.
```bash
sudo rm -rf /lib/modules/6.9.0-rc2 <your_kernel>
```
Just be aware my other commands are for my kernel, which happens to be named `"6.9.0-rc2"`. Just change the name in the command to the name you gave to your kernel.
```bash
sudo rm /boot/vmlinuz-6.9.0-rc2 /boot/initramfs-6.9.0-rc2.img
```
then update grub
```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
