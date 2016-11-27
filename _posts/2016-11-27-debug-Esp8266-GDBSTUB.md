---
published: false
layout: post
description: Basic Debuging on Esp8266
tags:
  - Esp8266
title: Basic Debuging on Esp8266
---
<h2>Introduction</h2>
It is necessary a way to debug your code in C in order to check it works properly. There are several alternatives however Espressif released its own libraries to do it. It is not perfect but really easy to integrate.

Add to your makefile file.
{% highlight makefile %}
ifeq ($(ENABLE_GDB), 1)
	CFLAGS += -Og -ggdb -DGDBSTUB_FREERTOS=0 -DENABLE_GDB=1
	MODULES		 += $(SMING_HOME)/gdbstub
	EXTRA_INCDIR += $(SMING_HOME)/gdbstub
else
	CFLAGS += -Os -g
endif
{% endhighlight %}

{% highlight c %}
#ifdef ENABLE_GDB
	#include "../gdbstub/gdbstub.h"
#endif
 
extern void init();
 
extern "C" void  __attribute__((weak)) user_init(void)
{
	system_timer_reinit();
	uart_div_modify(UART_ID_0, UART_CLK_FREQ / 115200);
	cpp_core_initialize();
	System.initialize();
#ifdef ENABLE_GDB
	gdbstub_init();
#endif
	init(); // User code init
}
{% endhighlight %}

<h2>Installing GDB in Eclipse</h2>


<h3>References</h3>
1. <a href="https://github.com/espressif/esp-gdbstub" target="_blank">Esp GdbStub</a>
2. <a href="https://blog.attachix.com/live-debugging-with-open-source-tools-programming-for-esp8266-part-4/" target="_blank">Esp GdbStub</a>
