# Jaguar XF Canbus Decoding/linux integration

This project is a guide on how to install android auto (open auto pro) into a freelander 2, currently it is very basic with no real integration to the car, the plan is to go the same route I am going with my jaguar XF and integrate it to the CAN bus controls, open auto pro allows linking of other apps, so some plans for this will include visual representation of parking sensors, similar to what the jag has, and perhaps some obd information into a dashboard display.

If anyone wants to help with some canbus decoding the procedure will be exactly the same as it is in the jaguar, details/guide can be found on the jaguar branch or * [here](https://github.com/rhysmorgan134/JaguarXf_CAN)

(Hardware below shows set up for raspberry pi 3, there is now the improved 4 out, but there are issues with the genuine touchscreen, when this is resolved, I will update the guide/links, however the pi3 is more than capable and what I have used in both the jag and FL2)

## Hardware

* [RaspberryPi](https://www.amazon.co.uk/gp/product/B07BDR5PDW/ref=as_li_tl?ie=UTF8&camp=1634&creative=6738&creativeASIN=B07BDR5PDW&linkCode=as2&tag=rhysmorgan134-21&linkId=d3404d6b50251166481d1b5ee8acb8a8)
* [Cigarette USB Power 2.4A](https://www.amazon.co.uk/gp/product/B00Q2GFP4M/ref=as_li_tl?ie=UTF8&camp=1634&creative=6738&creativeASIN=B00Q2GFP4M&linkCode=as2&tag=rhysmorgan134-21&linkId=0ceffff1f473775468b01656e9b55c2d)
* [3.5mm Aux](https://www.amazon.co.uk/gp/product/B01F24EZM2/ref=as_li_tl?ie=UTF8&tag=rhysmorgan134-21&camp=1634&creative=6738&linkCode=as2&creativeASIN=B01F24EZM2&linkId=682e515b882ff5723e5435a8aa02017a)
* [Micro Usb Cable](https://www.amazon.co.uk/gp/product/B019Q6Y2MK/ref=as_li_tl?ie=UTF8&tag=rhysmorgan134-21&camp=1634&creative=6738&linkCode=as2&creativeASIN=B019Q6Y2MK&linkId=60281c9f2afb1782a6089aeeddf6093b)
* [official Pi Screen](https://www.amazon.co.uk/gp/product/B014WKCFR4/ref=as_li_tl?ie=UTF8&tag=rhysmorgan134-21&camp=1634&creative=6738&linkCode=as2&creativeASIN=B014WKCFR4&linkId=eea68be93280562ce100b096f32b65a2)
* [Most Bus Loop Connector](https://www.amazon.co.uk/gp/product/B07GPYMWX1/ref=as_li_tl?ie=UTF8&tag=rhysmorgan134-21&camp=1634&creative=6738&linkCode=as2&creativeASIN=B07GPYMWX1&linkId=67faafaa0cdb1d6c25ba78cb9deb0bbf)
* [USB soundcard](https://www.amazon.co.uk/gp/product/B01N905VOY/ref=as_li_tl?ie=UTF8&tag=rhysmorgan134-21&camp=1634&creative=6738&linkCode=as2&creativeASIN=B01N905VOY&linkId=6f9d3133a223079c16ef0dc71febf88a)


## Installing

I purchased a second fascia so that if needed I could modify it, it turns out the gap between the two airvents are excruciatingly close to being perfect, however they aren't. What I have done (as I have a spare fascia) is to remove the airvents by popping the clips either side

![Alt text](/images/clips.jpg?raw=true "Title")

you then need to trim all the bits of plastic which prevent the screen from sitting flush with the screen cut out, leave the tabs that are highlighted as green, these provide a nice way to level the screen up

![Alt text](/images/tabs.jpg?raw=true "Title")

With these out you need to cut along the redline, I used a dremel, this is the part where you have to cut all the way to the top, which means there is a visible cut mark once assembled inside of the air vent, as I purchased a spare fascia, I now have my old un altered one, I am thinking that it may be possible to just cut a slit in this plastic that the screen slides into so that there isnt a hole inside the airvent, I will look into this when I get another screen and update here, or if someone wants to try it and feedback that would be good.

![Alt text](/images/cutLines.jpg?raw=true "Title")

your screen should now sit flush against the cut out with not gap, to mount the screen I made use of the supports highlighted in blue in this picture, if the screen rests against these then its a bit too low and the bottom of the screen gets cut off, so I epoxy'd some m5 nuts onto them which made it sit perfectly, to hold the screen against the cut out, I first drilled a whole in the green circle tab highlighted in green two pictures above, and then linked some cable ties together, ran over the top of the screen, and then through the green circle in the picture below. For now this is holding it very securely, but now I have the old screen out I am going to looking 3d printing some stand offs to allow the screen to mount that that (watch this space).

![Alt text](/images/supports.jpg?raw=true "Title")

It should now be fairly strongly mounted (albeit rather rudimentary) and the fascia is ready to fit (do the software first and fully test before fitting)

## Software

* [Free Android Auto](https://github.com/f1xpl/openauto)
* [Open Auto Pro](https://bluewavestudio.io/index.php/bluewave-shop/openauto-pro-detail)

There are 2 routes you can go with the software, if you just want to test on a pi you got sitting around, then you can build open auto from source and give it a go, this means you will need to have raspbian set up on your pi, and have a basic understanding of terminal commands, the link to this software is above, and there are some youtube tutorials online on how to get it going, this software isnt frequently updated and you may need to do some fixes to get it working.

If you want something more permanent then go for open auto pro, the license isnt much, and its kept upto date, the software comes as a raspberry pi image, so you just flash it on the sd card, insert it, boot the pi up and away it goes, it also contains a wrapper around android auto that can do hands free and some other cool stuff. Going this route does mean you need a usb sound card (I used the link above) but the raspberry Pi sound card is known for being pretty poor.


## Modifications

One issue with running the Pi in the car, is the problem of getting a graceful shut down, similar to PCs the pi really needs to be shutdown and not the power just cut to it. The best fix for this is using 2 relays, and one timed relay. But I have implemented a little workaround for now until I can get to that.

Its done using a script, basically the script detects when an android phone gets plugged in, when the phone is then removed, it sends a shutdown signal to the pi. the logic here is that you plug your phone in when you get in the car, then once you finish your journey, you unplug the phone, which shuts the Pi down, you then turn off your engine (the FL2 even keeps power to the switch ignition for around 5 seconds which should allow for the pi to shut down.

to add my script in its pretty simple, SSH to the pi, or attach a key board and use the screen.

run the following commands:

```cd Documents```

```nano usbMon.py```

then paste the following text in, make sure the indentation is the same 

```import subprocess
import os
import time

phoneConnected = 0
lastCycle = 0
shutDown = False

while not shutDown:
        time.sleep(1)
        df = subprocess.check_output("lsusb")
        devices = []
        if "Android" in df:
                phoneConnected = 1
        else:
                phoneConnected = 0
        if phoneConnected == 0 and lastCycle == 1:
                os.system("sudo shutdown -h now")
                shutDown = True
        else:
                lastCycle = phoneConnected
```

to exit press ctrl + x then when prompted press y to save

now put in the command

```sudo nano /etc/rc.local```

then inside this file comment out the line that says 

```/usr/bin/hcitool cmd 0x3F 0x01C 0x01 0x02 0x00 0x01 0x01```

comment it out by putting a # in front of it, so the line should look like

```#/usr/bin/hcitool cmd 0x3F 0x01C 0x01 0x02 0x00 0x01 0x01```

then on the line above it paste

```sudo python /home/pi/Documents/usbMon.py &```

this runs the script on startup, ensure you add the & sign at the end, otherwise your raspberry pi wont boot and you will have to reflash the memory card. Again exit out by pressing ctrl + x, and then y at the prompt.

Once this is done test it, if you are happy with it and the pi shuts down once you unplug your phone, then take an image of your sd card using your pc. this means if your sd card gets corrupted you can just reload the image back on in 5 minutes.

##conclusion

For now thats all there is too it, but in the coming weeks I will be starting to look into the canbus, and then we can get some pretty cool stuff added, steering wheel controls, park aid, car parameters etc etc.

Any questions you can open an issue on here, or you can get me on the freelander 2 facebook group.
