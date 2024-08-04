# SDR with RTL-SDR on a Linux VM with GQRX
After verifying that my hardware worked, it's time to create a Linux VM that can handle the various things I would like to try that are only available on Linux.

## Create a VM
The first step of this is to create the VM. I have another post on doing this just to get a browser running. (have not yet moved this to GitHub) So here I will only focus on what is different for this VM. These are the settings used for this one:

* 4 processors (needs a bit more computing that just a browser)
* 16GB memory (again, need more than just a browser)
* 30GB hard drive (in case I need that much space, but set to expandable to keep the actual size down)
* Shared folder configured to an SSD drive on the host (in case it needs to be faster to log radio traffic)
* Installed the VirtualBox extension pack to provide USB 3.0 to the VM
* Added a USB Device Filter (mine showed as RTL2838UHIDIR). Set the VM to use USB 3.0
* Installed Ubuntu Linux, same as the other post, but didn’t remove as many software packages and didn’t install Chrome, just left FireFox

## Get drivers and software
When this system boots, you can hear the sound for Windows dropping a USB device as the VM grabs that device. To get get the driver and install the package, you only need to use the regular install commands.
```
sudo apt update
sudo apt upgrade
sudo apt install rtl-sdr
sudo apt install gqrx-sdr
sudo apt autoremove
sudo apt clean
```
The first two are the standard machine update and the last two are to keep the VM size down. You may need to reboot at this point so the just installed driver can pick up the dongle. Then just run gqrx from the command line to start it, but be aware that it will create a config file for your device from the current directory, so you may wish to be in your home directory. The first time it runs you will need to select your device in the pop-up window and it should be named something with ‘2838’ in the name. The default settings are fine for the rest.

To see something in the FM range, do the following:

1. Go to the Receiver Options (you might just see ‘Receiver…’ in the tab) tab on the right-hand-side configuration area.
2. Set the Frequency to 105.7MHz (or another for your area). I could not pick up 106.5 with Gqrx, don’t know why and could not improve it.
3. Set the Mode to WFM (mono)
4. Press the play button under the File menu (mine looks like a right pointing triangle). It worked for me even though it looked grayed out the entire time.

If we heard music, the test was successful. Some notes on this part:

* Gqrx did not seem as turn-key as SDR#. It didn’t show the peak for 106.5 and didn’t have the nice decoding of the sub-messaging, like the station name.
* There were a lot of errors coming out of the terminal window and some searching seems to say they are mostly noise (see below).
* I managed to break Gqrx at one point and needed to uninstall and reinstall the rtl-sdr drivers to get things back.
* If a bad config keeps it from starting, you can move or delete the old config located at /home/USERNAME/.config/gqrx where ‘USERNAME’ is your username.
* After installing rtl-sdr you should be able to run rtl_test. I was getting some errors, but it still seemed to be working. Strange.
* I installed the VirtualBox extension pack and then selected the USB filter for the VM to be USB 3.0 and the errors went away while running rtl_test. It still didn’t help Gqrx pick up 106.5
