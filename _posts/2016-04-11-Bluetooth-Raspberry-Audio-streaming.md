In this post I would like to show how to install a bluetooth dongle to stream music from the Raspberry pi to bluetooth loud speakers. As I didn't have bluetooth loud speakers I bought a cheap receiver bluetooth in eBay to stream music.

<center><img class="alignnone" src="/images/bluetoothDongle.jpg"/></center>

<!-- more -->

<h2>1) Characteristics</h2>
Based on the sellers mail this is what this little bluetooth can do. This bluetooth is to be connected to the LOUD SPEAKERS, no to the Raspberry. For the Raspberry you need another bluetooth dongle.

{% highlight bash %}
Bluetooth Standard: Bluetooth V2.0+EDR, support A2DP V1.2
Power supply: 5 V USB
Output: 3.5mm Audio interface
Distance: about 10m
Sound output rate: 44.1kHZ and 48 kHz
Pairing code: 0000
{% endhighlight %}

<h2>2) Installation</h2>
First of all you need to detect if the bluetooth dongle in your Raspberry is properly detected.
{% highlight bash %}
lsusb
{% endhighlight %}

In my case I get.
{% highlight bash %}
Bus 001 Device 004: ID 0a12:0001 Cambridge Silicon Radio, Ltd Bluetooth Dongle (HCI mode)
{% endhighlight %}

After that you have to install some tools to set up the raspberry
{% highlight bash %}
sudo apt-get install bluetooth bluez bluez-utils bluez-alsa
{% endhighlight %}

Add the default user "gusa" to the lp group (you will be able to see bluetooth sources and to change some bluetooth settings) :
{% highlight bash %}
sudo usermod -a -G lp gusa
{% endhighlight %}

Change the default bluetooth audio settings :
{% highlight bash %}
sudo nano /etc/bluetooth/audio.conf
{% endhighlight %}

Add/Complete the following line in the [General] section :
{% highlight bash %}
Enable=Source,Sink,Socket
Disable=Media
{% endhighlight %}

Done it reboot the system.
{% highlight bash %}
sudo reboot
{% endhighlight %}

Make the usb dongle works:
{% highlight bash %}
sudo hciconfig hci0 up
{% endhighlight %}

Scan for your bluetooth loud speakers :
{% highlight bash %}
hcitool scan
{% endhighlight %}

Once it is done you will get a mac address or BT ADD or BlueTooth Address of the bt device. Your Bluetooth device will have a different id.
{% highlight bash %}
Scanning ...
	00:11:67:8C:17:80	H163
{% endhighlight %}

This command will show you your local mac address.
{% highlight bash %}
hcitool dev
{% endhighlight %}

In my case
{% highlight bash %}
hci0	00:02:5B:51:19:C0
{% endhighlight %}

Next, you will use bluez-simple-agent to pair the device. My device can be paired with the passphrase 0000.
{% highlight bash %}
bluetooth-agent --adapter hci0 0000 00:11:67:8C:17:80 
{% endhighlight %}

Normally you can use this instead the last line I used:
{% highlight bash %}
bluez-simple-agent hci0 <hadware_id>
{% endhighlight %}

And then you will be asked:
{% highlight bash %}
RequestPinCode (/org/bluez/2211/hci0/dev_<hadware_id>)
Enter PIN Code: <pin_code>;
Release
New device (/org/bluez/2211/hci0/dev_<hadware_id>)
{% endhighlight %}

{% highlight bash %}
bluez-simple-agent hci0 00:11:67:8C:17:80
{% endhighlight %}

Now if you want to make the remote device be able to connect back to you without user authorization:
{% highlight bash %}
bluez-test-device trusted 00:11:67:8C:17:80 yes
{% endhighlight %}

Check if id is already trusted:
{% highlight bash %}
bluez-test-device trusted 00:11:67:8C:17:80
{% endhighlight %}

The result will be 0 for not trusted and 1 for trusted.
Now connect with alsa adding to YOUR user:
{% highlight bash %}
sudo nano  ~/.asoundrc
{% endhighlight %}

This content :
{% highlight bash %}
pcm.bluetooth {
        type bluetooth
        device 00:11:67:8C:17:80 # change this MAC address to the one you wrote down
        profile "auto"
}
{% endhighlight %}

<h2>3) Streaming music</h2>
Restart the bluetooth service to start streaming music:
{% highlight bash %}
sudo /etc/init.d/bluetooth restart
{% endhighlight %}

<h3>3.1) Mplayer</h3>
Finally just install for example Mplayer and play like :
You have to select audio output as Alsa device with the bluetooth you set up before. You have to resample the music to avoid fulling the buffer.
{% highlight bash %}
mplayer -ao alsa:device=bluetooth ./05\ Is\ It\ A\ Crime.mp3  --af=resample=22100:0:0
{% endhighlight %}

<h3>3.2) CMUS</h3>
CMUS is a music player for the terminal.
{% highlight bash %}
Sudo apt-get install cmus
{% endhighlight %}

In order to set up the Raspberry to run the audio using the alsa bluetooth device set up before open CMUS, press 7 and change this parameters like this.
{% highlight bash %}
dsp.alsa.device bluetooth
output plugin alsa
{% endhighlight %}

<h3>3.3) MPG321</h3>
Install mpg321, a command line music player:
{% highlight bash %}
sudo apt-get install mpg321
{% endhighlight %}

After installing, just play some music:
{% highlight bash %}
mpg321 -a bluetooth -g 15 yourMP3.mp3
{% endhighlight %}

Some explanation of the switches:
-a bluetooth : The audio device (the one we just add)
-g xx : The gain (audio volume You can change it in runtime with / and *)

<h2>4) Referenceces</h2>
1. <a href="http://www.mplayer2.org/docs/ao/" target="_blank">Mplayer audio output</a>
2. <a href="http://www.mplayer2.org/docs/af/" target="_blank">Mplayer resample option</a>
3. <a href="http://www.tuxarena.com/static/cmus_guide.php" target="_blank">CMUS tutorial</a>
4. <a href="https://cmus.github.io/" target="_blank">CMUS web page</a>
5. <a href="http://blog.whatgeek.com.pt/2014/04/20/raspberry-pi-bluetooth-wireless-speaker/" target="_blank">Raspberry PI Bluetooth Wireless Speaker</a>
