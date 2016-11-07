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
