---
published: false
layout: post
description: Basic FreeRtos Esp8266 serial port with pc
tags:
  - Esp8266 FreeRtos
---
<center><img src="/images/freeRtos.png" width="236" height="92"></center>

In this tutorial I will show how to set up a basic serial port with the Esp8266 and the pc. This tutorial is based on the examples of Esp Open Rtos framework that you can find  <a href="https://github.com/SuperHouse/esp-open-rtos/tree/master/examples" target="_blank">here</a>

<!-- more -->
 
<h2>Installation process</h2>
First of all you have to install Esp Open Sdk and Esp Open Rtos. After that you have to compile and flash the program inside the Esp. You can find explanation how to do it <a href="{{ site.baseurl }}{% post_url 2016-11-27-Esp8266-FreeRtos %}" target="_blank">here</a> 


<h2>Compiling and flashing</h2>
After installing everything just go inside esp-open-rtos/examples/simple and type: 
{% highlight bash %}
make flash
{% endhighlight %}
after booting Esp in flash mode. 

<h2> Code analysis </h2>
As you can see in this code it just create two tasks. The first one counts and send a message to the other one to be printed. Another thing to take in account is how to setup the serial port. Here you have to define the speed port (115200). 

{% highlight c %}
/* Very basic example that just demonstrates we can run at all!
 */
#include "espressif/esp_common.h"
#include "esp/uart.h"
#include "FreeRTOS.h"
#include "task.h"
#include "queue.h"

void task1(void *pvParameters)
{
    QueueHandle_t *queue = (QueueHandle_t *)pvParameters;
    printf("Hello from task1!\r\n");
    uint32_t count = 0;
    while(1) {
        vTaskDelay(100);
        xQueueSend(*queue, &count, 0);
        count++;
    }
}

void task2(void *pvParameters)
{
    printf("Hello from task 2!\r\n");
    QueueHandle_t *queue = (QueueHandle_t *)pvParameters;
    while(1) {
        uint32_t count;
        if(xQueueReceive(*queue, &count, 1000)) {
            printf("Got %u\n", count);
        } else {
            printf("No msg :(\n");
        }
    }
}

static QueueHandle_t mainqueue;

void user_init(void)
{
    uart_set_baud(0, 115200);
    printf("SDK version:%s\n", sdk_system_get_sdk_version());
    mainqueue = xQueueCreate(10, sizeof(uint32_t));
    xTaskCreate(task1, "tsk1", 256, &mainqueue, 2, NULL);
    xTaskCreate(task2, "tsk2", 256, &mainqueue, 2, NULL);
}
{% endhighlight %}

<h2>Testing the program</h2>
In order to test the program you have to install a serial port in your pc. In this case, for Linux I am going to use <a href="http://gtkterm.feige.net/" target="_blank">GTKTerm</a>. 

Install GTKTerm.
{% highlight bash %}
sudo apt-get install gkterm
{% endhighlight %}

Based on the example program port parameters are:
Port: /dev/ttyUSB0
Baud Rate: 115200
Parity: None
Bits: 8
StopBits: 1
Flow control: None

This is what you should see in your terminal.
{% highlight bash %}
blpp_task_hdl : 3fff1e38, prio:14, stack:512
pm_task_hdl : 3fff22c8, prio:1, stack:176
frc2_timer_task_hdl:3fff4e90, prio:12, stack:200

ESP-Open-SDK ver: 0.0.1 compiled @ Dec  4 2016 18:59:54
phy ver: 273, pp ver: 8.3

SDK version:0.9.9
mode : softAP(1a:fe:34:d2:ec:59)
add if1
bcn 100
Hello from task1!
Hello from task 2!
Got 0
Got 1
Got 2
Got 3
Got 4
Got 5
Got 6
Got 7
Got 8
Got 9
Got 10
{% endhighlight %}