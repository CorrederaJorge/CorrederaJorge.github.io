---
published: true
layout: post
tags: 
  - Ros
  - RaspberryPi
description: Installing Ros on Raspberry Pi
modified: {}
title: Installing Ros on Raspberry Pi
---



After playing a bit with Ros I discovered a really easy way to install Ros Hydro in Raspbian thanks to the guys of Ros Sig Embedded. They released a repo that you only have to add and install from it. Really easy. Let's see how.

<center> <img src="/images/rosHydro.png" alt="rosHydro" width="280" height="300" /></a></center>
<!--more-->

<h2>1) Installation</h2>
First of all you have to add the repos to your repo list:
{% highlight bash %}
deb http://64.91.227.57/repos/rospbian wheezy main
{% endhighlight %}

Add the repo key:
{% highlight bash %}
wget http://64.91.227.57/repos/rospbian.key -O - | sudo apt-key add -
{% endhighlight %}

After that just install from the repository:
{% highlight bash %}
sudo apt-get install ros-hydro-ros-comm
sudo apt-get install python-rosinstall
{% endhighlight %}

<h2>2) Creating a package</h2>
In order to create a new Ros Package use the next <a href="http://wiki.ros.org/catkin/Tutorials" target="_blank">tutorial</a>.

When I tried to compile the package I saw that the libraries where not available, a quick way to fixe this problem was to create a simbolik link where Ros looks for the libraries. I need to go further for this problem because there must be a place where it could be possible to:
{% highlight bash %}
sudo ln -s /usr/lib/arm-linux-gnueabihf/libboost_signals.so /usr/lib/libboost_signals-mt.so
sudo ln -s /usr/lib/arm-linux-gnueabihf/libboost_filesystem.so /usr/lib/libboost_filesystem-mt.so
sudo ln -s /usr/lib/arm-linux-gnueabihf/liblog4cxx.so  /usr/lib/liblog4cxx.so
sudo ln -s /usr/lib/arm-linux-gnueabihf/libboost_regex.so /usr/lib/libboost_regex-mt.so
sudo ln -s /usr/lib/arm-linux-gnueabihf/libboost_date_time.so /usr/lib/libboost_date_time-mt.so
sudo ln -s /usr/lib/arm-linux-gnueabihf/libboost_system.so /usr/lib/libboost_system-mt.so
sudo ln -s /usr/lib/arm-linux-gnueabihf/libboost_thread.so /usr/lib/libboost_thread-mt.so
{% endhighlight %}

<h2>3)Where to go from here</h2>
I need to complete this small tutorial in order to create a Catkin Workspace and a basic package.
<h2>4)References</h2>

1. <a title="Ros sig Embedded" href="http://wiki.ros.org/sig/Embedded" target="_blank">Ros sig Embedded</a>
2. <a title="Error when linking libraries" href="http://answers.ros.org/question/9338/cannot-find-libraries-when-linking/" target="_blank">Error when linking libraries</a>
3. <a title="Creating Ros Package" href="http://wiki.ros.org/catkin/Tutorials/CreatingPackage" target="_blank">Creating Ros Package</a>
