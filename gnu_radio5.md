# Gnu Radio - Moving from RTL-SDR to HackRF One
My HackRF One arrived. So just plug it in and run, right? Nope. Since I am running on Windows 10 with a Ubuntu Linux Virtual Box VM, this is what I had to do to get going, roughly in order:

1. You need to uninstall the HackRF One from the Windows Device Manager.
2. Then install drivers using zadig.exe.
3. Then I had to install SDR Plus Plus as SDRSharp was not letting me change the gain.
4. Then set the gain way up by both turning on the yes/no selector and the slider to 20. Then I got something similar to what RTL-SDR would give in Windows to turn in an FM station. Note that there is an odd peak that does not work at wherever the center frequency is set to. You can tune in to another station that shows up in the range.
5. Now that it seems the hardware is up on the native OS. You will need to configure a USB pass-through on the VM, similar to what was covered in a previous post (just click the button to add a new device and choose the Hackrf. Also make sure you installed the extensions and have USB set to 3.0 for the VM)
6. We should be good, but test by running hackrf_info on the command line and make sure it sees your device. Then bring up Gnu Radio and make a copy of the test flow for FM radio used previously.
7. You will need to replace the RTL-SDR source with an osmocon one and then set the gain to both Ch0: Gain Mode: to True and Ch0: RF Gain (dB): to 20
8. The layout should now run OK except that the center frequency has a spike and is unusable, but things near by work. Also at this point, I can’t stop the run, change a setting and restart as that locks up the hardware.

So… the hardware is being weird. Maybe I should test on Pentoo like the videos say. Don’t do it. Pentoo does not secure boot without a ton of work (ironic). It also does not have any of the needed software on the USB image by default (wasn’t that the reason to try it). There was no release version when I went to try it so I downloaded the latest. If you turn off all boot security, you can boot to Pentoo, however, you have to set a complex password (all I need to do is install a driver, but Pentoo insists). Is Pentoo really 'hardened' if you have to download and run unsigned software as admin to make your bootable USB? Maybe you can update the firmware from Windows, pretty much no. Stick with the thing you were advised against in the videos and update from the VM with Ubuntu? This worked:

1. Download the new firmware from [here](https://github.com/greatscottgadgets/hackrf/releases) in your Ubuntu VM. Odd that I could not browse there from the github repo
2. Expand the package somewhere, go there and run the command as outlined [here](https://hackrf.readthedocs.io/en/latest/updating_firmware.html)
```
hackrf_spiflash -w hackrf_one_usb.bin
```
3. At this point the firmware was updated (in my case from a 2018 version to a 2021 one), but its ID had changed. So I shutdown the VM and unplugged and replugged the HackRF to reboot it.
4. Since the ID changed, you will need to delete and recreate your USB passthrough device (like in step 5 above) and start the VM.
5. Test that it is working and has the updated software with the hackrf_info command, which will also tell you the firmware version.

Now the HackRF should work for multiple runs with Gnu Radio.
