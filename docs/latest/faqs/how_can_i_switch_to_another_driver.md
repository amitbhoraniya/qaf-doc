---
title: How can I switch to another driver during same testcase execution.
sidebar: faq_sidebar
permalink: latest/how_can_i_switch_to_another_driver.html
folder: latest
---

In some exceptional changes, it may be required to change the driver.

**For example** if you want to switch to any new driver while executing on any other driver, call the following steps.

```java
By changing any of these property existing driver teardowns and new driver instance creates
	driver.name
	driver.additional.capabilities
	remote.server
	remote.port

```			

 
