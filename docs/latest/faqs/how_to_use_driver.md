---
title: How to use driver?
sidebar: faq_sidebar
permalink: latest/how_to_use_driver.html
folder: latest
---

Default available driver names in QAF:

* firefoxDriver
* iExplorerDriver
* chromeDriver
* operaDriver
* androidDriver
* iPhoneDriver
* appiumDriver.

**Available Driver** : To use available driver, provide driver class as capability.

Example:
To use firefoxDriver

```
driver.name=firefoxDriver
```


To use appiumDriver for android, 

```
driver.name=appiumDriver
appium.additional.capabilities={‘driverClass’:’io.appium.java_client.android.AndroidDriver’}
```


**Other Driver** : To use custom driver, provide driver class as capability.

Example:
To use PhantomJSDriver

```
driver.name=otherDriver
other.additional.capabilities={‘driverClass’:’io.appium.java_client.android.AndroidDriver’}
```

You can also use driverClass capabilities as [different ways](setting_driver_capabilities.html).



