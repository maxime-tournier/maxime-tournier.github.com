---
title: Android
---


# TWRP

(note: this may brick your phone and i'm not responsible for
it. backup your files first!)

1. download TWRP recovery image for your device
2. install Heimdall: 
```sh
sudo apt-get install heimdall-flash-frontend
```
3. turn phone off
4. start phone in download mode: `vol down + home + power`
5. start Heimdall: 
```sh
heimdall-frontend
```
6. flash the `RECOVERY` partition with the downloaded TWRP recovery
   image
   - note: the device will quickly reboot so **be prepared to hold
     `vol-up + home`** when clicking "start"

7. your phone should boot on TWRP recovery. you probably don't want to
   modify the system partition and do a complete backup on the first
   run.
   
   
   

