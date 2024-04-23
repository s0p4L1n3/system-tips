If you are looking to increase your disk size on your VM, please follow this guide.
This guide is intended if you already have LVM as file system.

## Increase disk size on Vsphere

  First, you need to increase the disk size on VMWare VSphere (Same applies to Proxmox or Any hypervisor)

  <img width="404" alt="image" src="https://github.com/s0p4L1n3/system-tips/assets/126569468/4c2f265d-203f-4f46-bb1c-d015ba054791">
  <img width="638" alt="image" src="https://github.com/s0p4L1n3/system-tips/assets/126569468/2c3ad4fe-31ef-4e00-8187-48928897a41c">

## Increase disk size on VM

On your virtual machine, check information with `fdisk -l`.

1. Create the new LVM partition

`fdisk /dev/sda`
  - Type `n` to create new partition
  - Type `p` as it will be a primary partition
  - Type Enter twice to confirm the new size of partition (it will automatically allocate the free space)
  - Type `t` to change the partition ID
  - Type `x` where x is the number of the partition we created
  - Type `L` to list and check the number associated with `Linux LVM`
  - Type the number you found
  - Type `w` to apply the changes.

You may reboot the VM before continue.

2. Create the physical partition volume :
   
`pvcreate /dev/sda4`

4. Extend the volume groupe partition (check your vg name with `vgdisplay`):
   
`vgextend rhel9_tmp /dev/sda4`

6. Extend the desired logical volume (in my case I want to increase /root) (check your lv name with `lvdisplay`)
   
`lvextend -l +100%FREE /dev/rhel9_tmp/root`

8. Extend the file system with xfs:
   
`xfs_growfs /dev/rhel9_tmp/root`

You can look again with `fdisk -l`, you have now more capacity :)
