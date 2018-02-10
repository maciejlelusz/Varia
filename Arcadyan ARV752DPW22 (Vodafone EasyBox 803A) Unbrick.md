Ok. Simple and in few steps how to unbrick Arcadyan ARV752DPW22 (Vodafone EasyBox 803A) and put on it OpenWRT.

Firs Tread and understand this page https://wiki.openwrt.org/toh/astoria/arv752dpw22. Done? Don't understand a word? Don't worry I've been there too ;) This page is written by pros for pros. We, low level, shit eaters who not flash their boxes everyday need to read more.

Ok, stop bitching around. Let's flash this router. First thing - it's need to blink the lights, any of it. If it's not blinking... well you have big chance that your device is dead.

First connect to it via Seral console - you need cable, and drivers to your computer. I'm using mac, and for me the cheapest (2$) and the easiest was to choose usb to serial by profitec. When you buy it, and connect (AND INSTALL DRIVERS!!!!) you can connect to this shit by starting terminal, and then type:

`screen /dev/tty.usbserial 115200`

!!! REMEBER ADDRESSES IN THE MEMORY OF YOUR DEVICE YOU NEED TO OBTAIN BY YOUR OWN THIS ARE EXAMPLES!!!

Than if device is totaly boroken and only what you can see is U-Boot type:

`loady`

Than in your screen click Ctrl+A and type ":". This will allow you to upload new firmware. Firmware can be found here:

https://archive.openwrt.org/chaos_calmer/15.05.1/lantiq/xway/

When you will download firmware, you can type in screen window:

`exec !! lsz -X /path/to/your/file/with/firmware` (download *-uImage-initramfs)

Than your router should start to download software to RAM at some specific address, in my situation it was: 0x80100000

When it's finish your router will give you size of uploaded firmware (on the end of the line). This supposed look like this: 0x003b7c52 - this will be end of your file in RAM. So you have beginning 0x80100000 and end +0x003b7c52. 

So now it's a big moment. You delete what is now in Flash of router in the memory pages where you will want to put new firmware. Type:

`erase 0xB0040000 +0x003b7c52` (this is end of your file, with you have from uploading process)

After erase process finished you need to copy from RAM to Flash:

`cp.b 0x80100000 0xB0040000 0x003b7c52` (cp.b [RAM Address of the firmware] [FLash address of the firmware] [Size of firmware])

Than you check if everything is fine with the firmware:

`iminfo 0xB0040000'
saveenv`

Done! Your router should come back to live.

And you can boot into it:

`bootm 0xB0040000`

If your device after restart is now booting into new firmware you need to upgrade env variables to show where is located in Flash new firmware:

`set bootcmd 'bootm 0xB0040000'
saveenv`

Done! Your router should come back to live.
