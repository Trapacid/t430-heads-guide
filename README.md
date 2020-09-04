# t430-heads-guide
an actual guide for [heads](https://github.com/osresearch/heads) by osresearch on Lenovo ThinkPad T430

very quick overview:

1. dump 4mb and 8mb chips. **DUMP AGAIN AND CHECK FOR INTEGRITY. SAVE A BACKUP SOMEWHERE.**
2. `cat 8mb.bin 4mb.bin > 12mb.bin`
3. clone `corna/me_cleaner` and `osresearch/heads`
4. `cd heads; make` # compiles important stuff like ifdutil from coreboot
5. `cd build/coreboot-4.8.1/utils/ifdtool; cp ~/12mb.bin . ; ./ifdutil -u 12mb.bin`
6. `cp 12mb.bin.new ~/me_cleaner/`
7. `python3 me_cleaner.py -r -t -d -S -O clean_flash.bin 12mb.bin.new --extract-me extracted_me.rom`
8. `dd if=clean_flash.bin of=8mb_clean_flash.bin bs=1M count=8`
9. flash 8mb_clean_flash.bin to your 8mb chip. This has the IFD and ME regions unlocked for internal flashing and intel ME garbage neutered.
10. `cd ~/heads; make BOARD=t430` **check the CBFS region size in config/coreboot-t430.config. It MUST be 0x710000 otherwise it will not flash fully due to region sizes**; `make BOARD=t430-flash`
11. flash build/t430-flash/t430-flash.rom to your 4MB chip
12. create an ext4 USB drive to put build/t430/coreboot.rom onto. (you can use vfat/fat32 if you wanna be special, but ext4 probably more safer because of journaling)
13. boot up T430: `mount-usb; flash.sh /media/coreboot.rom`
14. `reboot`
15. enjoy. 

Tips:

Do not have internet, hard drives, or external media plugged in when you create your TOTP/HOTP and reset TPM.

If you are coming from stock Lenovo firmware, you will need to reset the TPM because the password is something no one knows [citation needed].

Sign your boot files after creating TOTP/HOTP and connecting your Linux hard drive (preferably Fedora)
