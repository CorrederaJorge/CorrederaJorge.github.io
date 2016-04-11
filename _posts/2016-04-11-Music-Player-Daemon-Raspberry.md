---
published: false
---

In this post I will show how to install a music server on the Raspberry to play music. The objective of this post is installing MPD with MPC or another front end to be able to control the music player and let it play.

<center>
<a href="http://www.correderajorge.es/wp-content/uploads/2014/06/streaming-MP41.jpg"><img class="aligncenter size-medium wp-image-1414" src="http://www.correderajorge.es/wp-content/uploads/2014/06/streaming-MP41-300x221.jpg" alt="Music" width="300" height="221" /></a></center>

<!-- more -->

<h1>1) Introduction</h1>
I am going to show how to install <a href="http://www.musicpd.org/" target="_blank">MPD</a> (Music Player Daemon) linked to the bluetooth speakers we set up before in the <a href="http://www.correderajorge.es/bluetooth-on-raspberry-audio-streaming/" target="_blank">previous post</a>.
<h2>2) MPD installation</h2>
First of all update all the system :
<div id="code">
<pre lang="bash" line="1">sudo apt-get update
sudo apt-get upgrade
</pre>
</div>
After that install MPD and <a href="http://www.musicpd.org/clients/mpc/" target="_blank">MPC</a> that :
<div id="code">
<pre lang="bash" line="1">sudo apt-get install mpd mpc alsa-utils
</pre>
</div>
As I showed in the previous post you have to install your bluetooth device as general user editing .asoundrc :
<div id="code">
<pre lang="bash" line="1">sudo nano .asoundrc
</pre>
</div>
So in my case :
<div id="code">
<pre lang="bash" line="1">   pcm.bluetooth {
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
</pre>
</div>
Now, once it is installed just type in the console this line to install mpd:
<div id="code">
<pre lang="bash" line="1">sudo gedit /etc/mpd.conf
</pre>
</div>
And set up it like this, set up your music home directory in <strong>music_directory</strong>, remember to modify the permission to let MPD read and write there. <strong> audio_output </strong> set up the audio output and points to the bluetooth configured in .asoundrc .
<div id="code">
<pre lang="bash" line="1">music_directory         "/home/gusa/musica"

bind_to_address         "localhost"

audio_output {
            type                    "alsa"
            name                    "bluetooth"
            device                  "bluetooth"     # optional
            format                  "22100:16:2"      # optional
}
</pre>
</div>
Allow the user mpd to access to the sound group :
<div id="code">gpasswd -a mpd audio</div>
<h3>2.1) Audio Jack output</h3>
In case you want to set up the output to the jack speaker in the Raspberry load the driver:
<div id="code">
<pre lang="bash" line="1">sudo modprobe snd_bcm2835
sudo amixer cset numid=3 1
</pre>
</div>
Load the module at the start of the system:
<div id="code">
<pre lang="bash" line="1">sudo nano /etc/modules
</pre>
</div>
<div id="code">
<pre lang="bash" line="1"># /etc/modules: kernel modules to load at boot time.
#
# This file contains the names of kernel modules that should be loaded
# at boot time, one per line. Lines beginning with "#" are ignored.
# Parameters can be specified after the module name.

snd-bcm2835
</pre>
</div>
And finally in the mpd.conf file :
<div id="code">
<pre lang="bash" line="1">audio_output {
        type            "alsa"
        name            "My ALSA Device"
        device          "hw:0,0"        # optional
#       format          "44100:16:2"    # optional
#       mixer_device    "default"       # optional
#       mixer_control   "PCM"           # optional
#       mixer_index     "0"             # optional
}
</pre>
</div>
<h2>3) MPC</h2>
<a href="http://www.musicpd.org/clients/mpc/" target="_blank">MPC</a> is a client for MPD. In order to kill mpd server just type :
<div id="code">
<pre lang="bash" line="1">sudo mpd ÔÇôkill
</pre>
</div>
If you want to add new content to the playlist use this line where / is the path to the music folder:
<div id="code">
<pre lang="bash" line="1">mpc add /
</pre>
</div>
In order to load a new playList type:
<div id="code">
<pre lang="bash" line="1">mpc load yourPlayList
</pre>
</div>
And now play music with:
<div id="code">
<pre lang="bash" line="1">mpc play
</pre>
</div>

<h2>4) References</h2>
a) <a href="http://www.forum-raspberrypi.de/Thread-tutorial-music-player-daemon-mpd-und-mpc-auf-dem-raspberry-pi" target="_blank">[Tutorial] Music Player Daemon (MPD und MPC) auf dem Raspberry Pi</a>
b) <a href="http://aubreykloppers.wordpress.com/2013/11/29/raspberry-pi-rompr-a-good-looking-web-based-music-player/" target="_blank">Raspberry Pi ÔÇô RompR (A good looking web-based music player) </a>
