---
published: true
layout: post
description: Use Orange pi Pc to create Jenkins CI server
tags:
  - TDD
---
Some time ago I decided to buy a Orange Pi pc. It is really close to Raspberry pi, however it is much cheaper. The objective of this small server is to be used as continuous integration server installing on it Jenkins and another tools.


<center><img src="/images/orangepi.png" width="300" height="300"></center>
<!-- more -->

<h3>Introduction</h3>
Based on <a href="https://en.wikipedia.org/wiki/Continuous_integration" target="_blank">wikipedia post</a>  CI is: In software engineering, continuous integration (CI) is the practice of merging all developer working copies to a shared mainline several times a day. In XP, CI was intended to be used in combination with automated unit tests written through the practices of test-driven development. Initially this was conceived of as running all unit tests in the developer's local environment and verifying they all passed before committing to the mainline. This helps avoid one developer's work-in-progress breaking another developer's copy. If necessary, partially complete features can be disabled before commit, such as by using feature toggles.

<h3>Hardware</h3>
There are some alternatives to <a href="http://www.orangepi.org/" target="_blank">Orange pi</a>  like <a href="https://www.raspberrypi.org/" target="_blank">Raspberry pi</a>. I decide to use Orange pi because is cheaper, however Raspberry pi alternative is better documented an easier to use. It is up to you to decide. 

Main charactersitics from  <a href="http://www.orangepi.org/orangepipc/" target="_blank">Orange pi</a> are:
 <table class="table table-bordered table-hover">
  <tr>
    <td  colspan="3" ><h3 >Hardware&nbsp;specification </h3></td>
  </tr>
  <tr>
    <td><p>CPU </p></td>
    <td  colspan="2" ><p>H3 Quad-core Cortex-A7 H.265/HEVC 4K</p></td>
  </tr>
  <tr>
    <td><p>GPU </p></td>
    <td  colspan="2" ><p >·Mali400MP2 GPU @600MHz<br>·Supports OpenGL ES 2.0</p></td>
  </tr>
  <tr>
    <td  ><p >Memory&nbsp;(SDRAM) </p></td>
    <td  colspan="2" ><p >1GB DDR3 (shared with GPU)</p></td>
  </tr>
  <tr>
    <td  ><p >Onboard&nbsp;Storage </p>
      </td>
    <td  colspan="2" ><p >TF card (Max. 64GB) / MMC card slot </p></td>
  </tr>
  <tr>
    <td><p >Onboard&nbsp;Network </p></td>
    <td  colspan="2" ><p><span>10/100M</span> Ethernet RJ45</p></td>
  </tr>
  <tr>
    <td  ><p >Power&nbsp;Source </p></td>
    <td valign="top" colspan="2" ><p >DC input can supply power, but USB OTG input don’t supply power</p></td>
  </tr>
  <tr>
    <td  ><p >USB&nbsp;2.0&nbsp;Ports </p></td>
    <td valign="top" colspan="2" ><p >Three USB 2.0 HOST, one USB 2.0 OTG </p></td>
  </tr>
</table>
<h3>Software</h3>
There are plenty of OS that you can install, in my case I decided to use ARMBian. As a CI server I will use Jenkins. It fits my requirements and I am used to it. In order to install Jenkins Java must be installed before. 
<h4>Os</h4>
In order to use <a href="http://www.armbian.com/orange-pi-pc/" target="_blank">ARMBian</a> you have to download the image and copy in to a SD card. I had some problems because I tried with different software to do it. The only one that worked properly was <a href="https://sourceforge.net/projects/win32diskimager/" target="_blank">Win32diskManager</a>. 

After that you have to boot the system and update it:
{% highlight bash %}
sudo apt-get update
sudo apt-get upgrade
{% endhighlight %}

<h4>Jenkins</h4>

First of all Java must be installed:
{% highlight bash %}
su -
echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" | tee /etc/apt/sources.list.d/webupd8team-java.list
echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886
apt-get update
apt-get install oracle-java8-installer
exit
{% endhighlight %}

if you want to set Oracle Java 8 as default:
{% highlight bash %}
sudo apt-get install oracle-java8-set-default
{% endhighlight %}

Check java version installed:
{% highlight bash %}
java -version
{% endhighlight %}

Finally install <a href="https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu" target="_blank">Jenkins</a>:
{% highlight bash %}
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
{% endhighlight %}

Upgrade and install:
{% highlight bash %}
sudo apt-get update
sudo apt-get install jenkins
{% endhighlight %}

Now you can access to your server ip using 8080 port.

<h3>References</h3>
1. <a href="http://www.webupd8.org/2014/03/how-to-install-oracle-java-8-in-debian.html" target="_blank">Install Java8</a>
2. <a href="https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu" target="_blank">Install Jenkins</a>
