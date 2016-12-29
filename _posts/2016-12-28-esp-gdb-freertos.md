---
published: false
---
<center><img src="/images/gdb-logo.png" width="300" height="193"></center>
One thing that Esp didn't have yet is a proper debug tool. The objective of this tutorial is showing how to make a proper debug based on Espressif and Vlad Ivanov (<a href="https://github.com/resetnow" target="_blank">resetnow</a>) tools.

<!-- more -->

<h2>First steps</h2>
- <a href="{{ site.baseurl }}{% post_url 2016-11-21-esp8266-openSdk %}" target="_blank">Install Esp Open Sdk</a> 
- <a href="{{ site.baseurl }}{% post_url 2016-11-27-Esp8266-FreeRtos %}" target="_blank">Install Esp Open Rtos</a> 

<h2>Esp Open Rtos GDBStub installation<h2>
In order to install GDB stub you have to follow the instructions from <a href="https://github.com/resetnow/esp-gdbstub" target="_blank">Esp GDBStub in github</a>.

Clone repository.
{% highlight bash %}
sudo git clone https://github.com/resetnow/esp-gdbstub
{% endhighlight %}

You have to install premake5. I haven't found it in the Linux repositories so I have downloaded fron <a href="https://premake.github.io/download.html" target="_blank">here</a> and put in the same folder as the project.

After that you have to run Premake5 that will create a make file. --with-eor flag must point to Esp Open Rtos installation folder. 

{% highlight bash %}
premake5 gmake --with-eor=/home/workspace/esp-open-rtos
{% endhighlight %}

<h2>Proyect configuration<h2>
I am going to use basic blink project from Esp Open Rtos example. In this project you have Makefile file.

{% highlight makefile %}
PROGRAM=blink

# Add this line to add gdbstub library.
# This could change based on your project location

EXTRA_CFLAGS+=-I../../esp-gdbstub/include
EXTRA_LDFLAGS+=-L../../esp-gdbstub/lib

include ../../common.mk

# Add this line to add gdbstub library.
LIBS+=esp-gdbstub
{% endhighlight %}

<h2>Eclipse configuration</h2>

<h3>References</h3>
1. <a href="https://github.com/resetnow/esp-gdbstub" target="_blank">Esp GDBStub in github</a>
2. <a href="https://github.com/Espressif/esp-gdbstub" target="_blank">Espressif GDB</a>
