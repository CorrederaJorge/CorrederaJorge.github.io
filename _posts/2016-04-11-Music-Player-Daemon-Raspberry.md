---
published: true
layout: post
description: How to set up a music player daemon in Raspberry pi
tags: 
  - Raspberry pi
title: "Music Player Daemon on Raspberry"
---



In this post I will show how to install a music server on the Raspberry to play music. The objective of this post is installing MPD with MPC or another front end to be able to control the music player and let it play.

<center><img class="alignnone" src="/images/streamingLogo.jpg"/></center>

<!-- more -->

<h1>1) Introduction</h1>
I am going to show how to install <a href="http://www.musicpd.org/" target="_blank">MPD</a> (Music Player Daemon) linked to the bluetooth speakers we set up before in the <a href="/Bluetooth-Raspberry-Audio-streaming/" target="_blank">previous post</a>.
<h2>2) MPD installation</h2>
First of all update all the system :
{% highlight bash %}
sudo apt-get update
sudo apt-get upgrade
{% endhighlight %}

After that install MPD and <a href="http://www.musicpd.org/clients/mpc/" target="_blank">MPC</a> that :
{% highlight bash %}
sudo apt-get install mpd mpc alsa-utils
{% endhighlight %}

As I showed in the previous post you have to install your bluetooth device as general user editing .asoundrc :
{% highlight bash %}
sudo nano .asoundrc
{% endhighlight %}

So in my case :
{% highlight bash %}
  pcm.bluetooth {
      type plug
      slave {
            pcm {
                type bluetooth
                device 00:11:67:8C:17:80
                profile "auto"
            }
      }
      hint {
           show on
           description "My bluetooth headset"
      }
   }

   ctl.bluetooth {
      type bluetooth
   }
{% endhighlight %}

Now, once it is installed just type in the console this line to install mpd:
{% highlight bash %}
sudo gedit /etc/mpd.conf
{% endhighlight %}

And set up it like this, set up your music home directory in <strong>music_directory</strong>, remember to modify the permission to let MPD read and write there. <strong> audio_output </strong> set up the audio output and points to the bluetooth configured in .asoundrc .
{% highlight bash %}
music_directory         "/home/gusa/musica"

bind_to_address         "localhost"

audio_output {
            type                    "alsa"
            name                    "bluetooth"
            device                  "bluetooth"     # optional
            format                  "22100:16:2"      # optional
}
{% endhighlight %}

Allow the user mpd to access to the sound group :
{% highlight bash %}
gpasswd -a mpd audio
{% endhighlight %}

<h3>2.1) Audio Jack output</h3>
In case you want to set up the output to the jack speaker in the Raspberry load the driver:
{% highlight bash %}
sudo modprobe snd_bcm2835
sudo amixer cset numid=3 1
{% endhighlight %}

Load the module at the start of the system:
{% highlight bash %}
sudo nano /etc/modules
{% endhighlight %}

{% highlight bash %}
# /etc/modules: kernel modules to load at boot time.
#
# This file contains the names of kernel modules that should be loaded
# at boot time, one per line. Lines beginning with "#" are ignored.
# Parameters can be specified after the module name.

snd-bcm2835
{% endhighlight %}

And finally in the mpd.conf file :
{% highlight bash %}
audio_output {
        type            "alsa"
        name            "My ALSA Device"
        device          "hw:0,0"        # optional
#       format          "44100:16:2"    # optional
#       mixer_device    "default"       # optional
#       mixer_control   "PCM"           # optional
#       mixer_index     "0"             # optional
}
{% endhighlight %}

<h2>3) MPC</h2>
<a href="http://www.musicpd.org/clients/mpc/" target="_blank">MPC</a> is a client for MPD. In order to kill mpd server just type :
{% highlight bash %}
sudo mpd kill
{% endhighlight %}

If you want to add new content to the playlist use this line where / is the path to the music folder:
{% highlight bash %}
mpc add /
{% endhighlight %}

In order to load a new playList type:
{% highlight bash %}
mpc load yourPlayList
{% endhighlight %}

And now play music with:
{% highlight bash %}
mpc play
{% endhighlight %}

<h2>4) References</h2>
1. <a href="http://www.forum-raspberrypi.de/Thread-tutorial-music-player-daemon-mpd-und-mpc-auf-dem-raspberry-pi" target="_blank">[Tutorial] Music Player Daemon (MPD und MPC) auf dem Raspberry Pi</a>
2. <a href="http://aubreykloppers.wordpress.com/2013/11/29/raspberry-pi-rompr-a-good-looking-web-based-music-player/" target="_blank">Raspberry Pi RompR (A good looking web-based music player) </a>
