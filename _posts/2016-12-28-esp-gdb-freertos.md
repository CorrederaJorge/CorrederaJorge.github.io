---
published: false
---
<center><img src="/images/gdb-logo.png" width="300" height="193"></center>
One thing that Esp didn't have yet is a proper debug tool. The objective of this tutorial is showing how to make a proper debug based on Espressif and Vlad Ivanov (<a href="https://github.com/resetnow" target="_blank">resetnow</a>) tools.

<!-- more -->

<h2>First steps</h2>
- <a href="{{ site.baseurl }}{% post_url 2016-11-21-esp8266-openSdk %}" target="_blank">Install Esp Open Sdk</a> 
- <a href="{{ site.baseurl }}{% post_url 2016-11-27-Esp8266-FreeRtos %}" target="_blank">Install Free Rtos</a> 

<h2>Freertos GDBStub installation<h2>
In order to install GDB stub you have to follow the instructions from <a href="https://github.com/resetnow/esp-gdbstub" target="_blank">Esp GDBStub in github</a>.

Clone repository.
{% highlight bash %}
sudo git clone https://github.com/resetnow/esp-gdbstub
{% endhighlight %}

You have to install premake5. I haven't found it in the Linux repositories so I have downloaded fron <a href="https://premake.github.io/download.html" target="_blank">here</a> and put in the same folder as the project.



<h2>Eclipse configuration</h2>

<h3>References</h3>
1. <a href="https://github.com/resetnow/esp-gdbstub" target="_blank">Esp GDBStub in github</a>
2. <a href="https://github.com/Espressif/esp-gdbstub" target="_blank">Espressif GDB</a>
