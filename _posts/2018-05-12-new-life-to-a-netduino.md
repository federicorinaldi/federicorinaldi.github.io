---
layout: recipe
title:  "Giving new life to a netduino with TinyCLR"
date:   2018-05-12 19:00:00 +1000
category: IoT
tags: 
    - TinyCLR
    - IoT
ingredients:
    - Netduino 3
    - TinyCLR config
    - TinyCLR VSIX
    - TinyCLR nuget packages
    - DfuSe
cook_time: "30 min"
image880x430: "cover880x430.png"
image1180x570: "cover1180x570.png"
image280x280: "2018-04-27-starting-a-blog-jekyll280x280.jpg"
---

I have a netduino 3 wifi that I bought a few years ago. As a .net developer, to have it running in every device possible is really apealing. Sadly this little device wasn't getting any love for the past few years so it was not possible to use it in VS 2017, you had to use it's SDK (a custom flavor of .Net Micro Framework) with VS 2015.

Now thanks to GHI (a great company and super .Net friendly) we can give a second life to this neat device, here is how to.
<!--more-->

## Steps to install

Firt let's download the TinyCLR firmware for netduino, GHI used to have a compiled version but it seemts that for today the latest release (v0.11.0) is not compiled anymore.

There are two ways to do this, let's start with the thorough one [(you can jump to the quick way if you want)](#quick-way)

### Build firmware from source

1. Clone the [TinyCLR-Ports repo](https://github.com/ghi-electronics/TinyCLR-Ports.git)
1. Download and install [GCC](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)
1. Download and extract the contents of [CMSIS](https://github.com/ARM-software/CMSIS_5/releases/download/5.2.0/ARM.CMSIS.5.2.0.pack) (it's a zip file, you can just rename it's extension) into the CMSIS folder of the cloned repo.
1. Download and extract the latest [TinyCLR OS Core Library](https://github.com/ghi-electronics/TinyCLR-Ports/releases) (as of this post v0.11.0) into the Core folder of the cloned repo.
1. Open a command prompt, change the directory to the cloned repo, and then execute:
{% highlight bash %}
build.bat netduino3
{% endhighlight %}
{:start="6"}
1. Now you should have inside Build/release/netduino3 an image called "netduino3 Firmware.hex"
1. Download [DfuSe USB device firmware upgrade](http://www.st.com/en/development-tools/stsw-stm32080.html#getsoftware-scroll) from STMicroelectronics
1. After installing the software, open DFU File manager and select "Generate a DFU from HEX"

![Generate a DFU from HEX](/assets/posts/2018-05-12-new-life-to-a-netduino/dfu-manager1.png)

{:start="9"}
1. Click S19 or Hex button and select the previously saved .hex firmware image
1. Select Generate and it will as you where tu put the generated .dfu file

![Generate a DFU](/assets/posts/2018-05-12-new-life-to-a-netduino/dfu-manager2.png)
<a name="quick-way"></a>

### Download firmware

If you find building from source too cumbersome, I already generated the image for you. Grab the generated DFU file [here](/assets/posts/2018-05-12-new-life-to-a-netduino/netduino3%20Firmware%20v0.11.0.dfu)

### Flash the device

Either if you built the image or you went on the easy route, you should have by now the .DFU file needed to flash the netduino.

1. If you don't have it already download [DfuSe USB device firmware upgrade](http://www.st.com/en/development-tools/stsw-stm32080.html#getsoftware-scroll) from STMicroelectronics.
1. Start the device in DFU (Device Firmware Upgrade) mode (hold down the onboard button on the Netduino and then plug into a USB port)
1. Open DfuSeDemo
1. Your device should appear in the dropdown list of "Available DFU Devices"
1. Under the Upgrade and Verify Action section, press the "Choose" button and select your .DFU image

![Upgrade firmware](/assets/posts/2018-05-12-new-life-to-a-netduino/dfuse.png)

{:start="6"}
1. Click "Upgrade"
1. After the upgrade finishes, click on Leave DFU Mode
1. Download [TinyCLR config](http://docs.ghielectronics.com/software/tinyclr/downloads.html#tinyclr-config). 

**Note: from now on make sure that everything you download is the same version as the firmware, if you downloaded the image I generated then the version is v0.11.0**

{:start="9"}
1. Select your device from the list and click Connect, your device should show the upgraded firmware version

### Hello world

Ok now that the boring part is over, we can start playing with our device. We are going to do the electronic equivalent to the hello world, a blinking led

1. Download the visual studio project system [here](http://docs.ghielectronics.com/software/tinyclr/downloads.html#visual-studio-project-system)
1. Download the Libraries [here](http://docs.ghielectronics.com/software/tinyclr/downloads.html#libraries)
1. Open VS and create a new TinyCLR application

![New TinyCLR application](/assets/posts/2018-05-12-new-life-to-a-netduino/vs1.png)

{:start="4"}
1. Right click on the solution and select Manage Nuget Packages for Solution
1. Add [Spirit.TinyCLR.Pins](https://www.nuget.org/packages/Spirit.TinyCLR.Pins)
1. Click on the settings icon

![Click on the settings icon](/assets/posts/2018-05-12-new-life-to-a-netduino/vs2.png)

{:start="7"}
1. Extract the libraries in a folder and this folder as a Package source

![Adding the folder as a package source](/assets/posts/2018-05-12-new-life-to-a-netduino/vs3.png)

{:start="8"}
1. Add GHIElectronics.TinyCLR.Devices and GHIElectronics.TinyCLR.Pins
1. Add the following code to your main method and run the app:

{% highlight csharp %}
static void Main()
{
    GpioPin led = GpioController.GetDefault().OpenPin(
    GHIElectronics.TinyCLR.Pins.Netduino3.GpioPin.Led);
    led.SetDriveMode(GpioPinDriveMode.Output);

    while (true)
    {
        led.Write(GpioPinValue.High);
        Thread.Sleep(100);
        led.Write(GpioPinValue.Low);
        Thread.Sleep(100);
    }
}
{% endhighlight %}

Now you should have a blinking light in your device

![Netduino blinking light](/assets/posts/2018-05-12-new-life-to-a-netduino/blinking-light.gif)

### Wrapping up

Once you have the device with TinyCLR installed, testing your software and deploying it to the device is pretty straight forward, sadly there are still things to improve as the lack of networking but being an open source project makes it easy for the comunity to be involved and takle this issues even if GHI won't.

I hope that as me, you can get your netduino from your drawer and start playing around with it with this new tools.