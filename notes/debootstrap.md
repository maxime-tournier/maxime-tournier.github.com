---
title: Debian
---

# Deboostrap

1. partition (and put labels while you're at it): `sudo gparted`
2. mount: `mount /dev/sd<id> /tmp/chroot`
3. boostrap: `sudo debootstrap <release> /tmp/chroot`
4. bind host fs to get network: 
   ```
   sudo mount --bind /dev /tmp/chroot/dev
   sudo mount -t proc proc /tmp/chroot/proc
   sudo mount -t sysfs sys /tmp/chroot/sys
   ```
4. chroot: `sudo chroot /tmp/chroot`
5. update: `apt-get update`
6. install text editor if you don't like vi: `apt-get install emacs`
7. edit fstab:
  1. obtain uuids: `blkid`
  2. fill `/etc/fstab` like so:
  ```
  UUID=75863510-edc6-4c60-89b0-875f8f53e0d6         /             ext4    defaults                 0    1
  UUID=9aa832a2-0be0-45af-a6e6-6e353d09bd9f	  none		swap	sw			 0    0

  proc		  /proc		proc	defaults		 0    0
  sys			  /sys		sysfs	defaults		 0	  0

  UUID=287f13a5-9497-4b48-b738-4c8eb94e7b86	  /home		ext4	rw,nosuid,nodev		 0    2
  ```

8. add user `adduser <username>`
   - add it to the `sudo` group `adduser <username> sudo

9. install a kernel: `apt-get install linux-image-generic`
   - this should prompt you with grub install device
   
10. configure locales: `dpkg-reconfigure locales`

11. install a desktop environment
   - update `/etc/apt/sources.list` to include `universe multiverse` on top of `main`
   - `apt-get install xubuntu-core`

12. reboot
