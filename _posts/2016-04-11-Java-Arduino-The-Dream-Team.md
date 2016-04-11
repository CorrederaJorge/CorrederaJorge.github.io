---
published: true
layout: post
description: "Connect Java and Arduino"
tags: 
  - Java
  - Arduino
title: "Java and Arduino : The Dream Team"
---

In this post I would like to show how to connect JAVA 8 and Arduino. My objective was being able to control some servos using Java, so for that it was necessary to use RXTX library. At first there was plenty of tutorials so it was easy, but it took me several hours to realize how to make it run properly. In conclusion here you are a basic how to with the main points

<center><img class="alignnone" src="/images/tutorial-rxtx.jpg"/></center>
<!-- more -->

<center><a><img class="aligncenter  wp-image-1468" src="http://www.correderajorge.es/wp-content/uploads/2015/01/tutorial-rxtx-300x87.jpg" alt="tutorial-rxtx" /></a></center>

<h2>1) Installing JAVA 8</h2>
The first step is installing JAVA 8, for that is we will use the repositories.
Add the repository.
{% highlight bash %}
sudo add-apt-repository ppa:webupd8team/java
{% endhighlight %}

Update the local repository.
{% highlight bash %}
sudo apt-get update
{% endhighlight %}

Install the package.
{% highlight bash %}
sudo apt-get install oracle-java8-installer
{% endhighlight %}

To set the default Java:
{% highlight bash %}
sudo apt-get install oracle-java8-set-default
{% endhighlight %}

<h2>2) Installing JAVA RXTX</h2>
The library that enables JAVA to communicate with Arduino is <a title="RXTX" href="http://rxtx.qbang.org/wiki/index.php/Main_Page" target="_blank">RXTX</a> . I had plenty of problems with that. For JAVA 8 the only one that worked was the JAVA RXTX 2.2 pre2. In order to install it just copy the files like this:
{% highlight bash %}
usr/lib/jvm/java-8-oracle/jre/lib/ext/RXTXcomm.jar
/usr/lib/jvm/java-8-oracle/jre/lib/amd64/librxtxSerial.so
{% endhighlight %}

Take in account that my pc is a 64 bits OS. You should change your folder and the *.so compilation in base of your system.
<h2>3) JAVA basic program</h2>
You should include the file RXTXcomm.jar in your project as a library in order to make your project run. A simple code example that you could use is based on the Arduino web page example:
{% highlight java %}
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.OutputStream;
import gnu.io.CommPortIdentifier; 
import gnu.io.SerialPort;
import gnu.io.SerialPortEvent; 
import gnu.io.SerialPortEventListener; 
import java.util.Enumeration;


public class SerialTest implements SerialPortEventListener {
	SerialPort serialPort;
        /** The port we're normally going to use. */
	private static final String PORT_NAMES[] = { 
			"/dev/tty.usbserial-A9007UX1", // Mac OS X
                        "/dev/ttyACM0", // Raspberry Pi
			"/dev/ttyUSB0", // Linux
			"COM3", // Windows
	};
	/**
	* A BufferedReader which will be fed by a InputStreamReader 
	* converting the bytes into characters 
	* making the displayed results codepage independent
	*/
	private BufferedReader input;
	/** The output stream to the port */
	private OutputStream output;
	/** Milliseconds to block while waiting for port open */
	private static final int TIME_OUT = 2000;
	/** Default bits per second for COM port. */
	private static final int DATA_RATE = 9600;

	public void initialize() {
                // the next line is for Raspberry Pi and 
                // gets us into the while loop and was suggested here was suggested http://www.raspberrypi.org/phpBB3/viewtopic.php?f=81&amp;t=32186
                System.setProperty("gnu.io.rxtx.SerialPorts", "/dev/ttyACM0");

		CommPortIdentifier portId = null;
		Enumeration portEnum = CommPortIdentifier.getPortIdentifiers();

		//First, Find an instance of serial port as set in PORT_NAMES.
		while (portEnum.hasMoreElements()) {
			CommPortIdentifier currPortId = (CommPortIdentifier) portEnum.nextElement();
			for (String portName : PORT_NAMES) {
				if (currPortId.getName().equals(portName)) {
					portId = currPortId;
					break;
				}
			}
		}
		if (portId == null) {
			System.out.println("Could not find COM port.");
			return;
		}

		try {
			// open serial port, and use class name for the appName.
			serialPort = (SerialPort) portId.open(this.getClass().getName(),
					TIME_OUT);

			// set port parameters
			serialPort.setSerialPortParams(DATA_RATE,
					SerialPort.DATABITS_8,
					SerialPort.STOPBITS_1,
					SerialPort.PARITY_NONE);

			// open the streams
			input = new BufferedReader(new InputStreamReader(serialPort.getInputStream()));
			output = serialPort.getOutputStream();

			// add event listeners
			serialPort.addEventListener(this);
			serialPort.notifyOnDataAvailable(true);
		} catch (Exception e) {
			System.err.println(e.toString());
		}
	}

	/**
	 * This should be called when you stop using the port.
	 * This will prevent port locking on platforms like Linux.
	 */
	public synchronized void close() {
		if (serialPort != null) {
			serialPort.removeEventListener();
			serialPort.close();
		}
	}

	/**
	 * Handle an event on the serial port. Read the data and print it.
	 */
	public synchronized void serialEvent(SerialPortEvent oEvent) {
		if (oEvent.getEventType() == SerialPortEvent.DATA_AVAILABLE) {
			try {
				String inputLine=input.readLine();
				System.out.println(inputLine);
			} catch (Exception e) {
				System.err.println(e.toString());
			}
		}
		// Ignore all the other eventTypes, but you should consider the other ones.
	}

	public static void main(String[] args) throws Exception {
		SerialTest main = new SerialTest();
		main.initialize();
		Thread t=new Thread() {
			public void run() {
				//the following line will keep this app alive for 1000 seconds,
				//waiting for events to occur and responding to them (printing incoming messages to console).
				try {Thread.sleep(1000000);} catch (InterruptedException ie) {}
			}
		};
		t.start();
		System.out.println("Started");
	}
}
{% endhighlight %}

<h2>3) Arduino CODE</h2>
In order to interact with Arduino it is necessary to upload some code to the board using the IDE.
{% highlight bash %}
 void setup(){
    Serial.begin(9600); //Config serial port
  }
 
void loop(){
// Check serial port availability
    if(Serial.available() > 0){
 
        inByte = Serial.read();
        Serial.println(inByte);
        if(inByte == '0')
      [...] // Do what ever you want 
    }
}
{% endhighlight %}

<h2>4) References</h2>
1. Library files that I have used <a href="http://www.correderajorge.es/wp-content/uploads/2015/01/rxtx-2.2pre2-bins.zip">rxtx-2.2pre2-bins</a>
2. <a href="http://playground.arduino.cc/Interfacing/Java">Arduino-PlayGround</a>
3. <a href="http://rxtx.qbang.org/wiki/index.php/Main_Page">JAVA RXTX</a>
4. <a href=" http://www.yourownlinux.com/2014/02/how-to-install-oracle-java-jdk-6-7-8-in-linux.html">JAVA installation</a>
