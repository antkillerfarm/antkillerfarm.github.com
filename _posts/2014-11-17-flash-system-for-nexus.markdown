---
layout: post
title:  Flash system for nexus device
category: technology 
---

# How to flash android binary for nexus 4 in ubuntu #

* Download the binary from google offical site: [android binary](https://developers.google.com/android/nexus/images "android binary").

* Unzip the dowloaded binary, then enter the uncompressed directory:

{% highlight bash %}
    tar zxvf occam-ktu84p-factory-b6ac3ad6.tgz 
    cd occam-ktu84p/
{% endhighlight %}

* Run these commonds in termial

1. Using the adb tool: With the device powered on, execute:

{% highlight bash %}
    adb reboot bootloader
{% endhighlight %}

To make the phone transfor into fastboot mode.

2. The phone also need be unlocked, execute:

   {% highlight bash %}
       fastboot oem unlock
   {% endhighlight %}

3. Finally, run flash-all.sh in the current directory, if there is the message "waiting for device" stuck in terminal, make sure run the script under the sudo mode.

# How to flash revocery mode binary #

Clock Work Mod (CWM) is probably most known so we will use that one. First, download the binary which fits your phone from [CWM site](https://www.clockworkmod.com/rommanager "CWM site"). For example, my nexus 4's fited one is recovery-clockwork-touch-6.0.4.7-mako.img. Then execute in bash:

{% highlight bash %}
    fastboot flash recovery recovery-clockwork-touch-6.0.4.7-mako.img
{% endhighlight %}

Now, the CWM recovery mode is okay now.

# Root Nexus 4 #

Download the superuser.zip from [CWM superuser](http://download.clockworkmod.com/superuser/superuser.zip "CWM superuser"). Use adb to push the downloaded superuser.zip into /ment/sdcard/ on the phone, execute:

{% highlight bash %}
    adb push superuser.zip /mnt/sdcard/
{% endhighlight %}
  
Then reboot the phone, change into the recovery mode, use the CWM's UI to select the superuser.zip to install it on the phone.
