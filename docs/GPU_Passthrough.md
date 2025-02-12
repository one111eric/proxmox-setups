# GPU Passthrough Steps

## Bios Changes: Enable VT and all  iommu setting

## Prox Changes:
/etc/default/grub file

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on iommu=pt”
```
	
/etc/modules

```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

/etc/modprobe.d/blacklist.conf

```
blacklist radeon
blacklist nouveau
blacklist nvidia
blacklist nvidiafb
blacklist snd_hda_intel
```
	
lspci | grep NVIDIA
lspci -n -s 41:00

echo “options vfio-pci ids=id1,id2 disable_vga=1”>/etc/modprobe.d/vfio.conf

update-initramfs -u -k all
update-grub
reboot

## Create VM
CPU host, DIsplay Virtio, Install Ubuntu


## Configure Ubuntu


sudo apt update && sudo apt dist-upgrade
sudo apt install openssh-client openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh

vi /etc/ssh/sshd_config
```
PermitRootLogin yes
PasswordAuthentication yes
```

sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo bash -c "echo options nouveau modset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo update-initramfs -u
sudo reboot

sudo ubuntu-drivers autoinstall


	

