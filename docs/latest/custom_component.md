---
title: Custom component
sidebar: qaf_latest-sidebar
permalink: latest/custom_component.html
folder: latest
tags: [java,component,locator,testdata]
---


To create a component you need to extend ```com.qmetry.qaf.automation.webdriver.QAFWebComponent```.

{% include inline_image.html file="CustomComponent1.png" alt="Custom Component " %} 

{% include inline_image.html file="CustomComponent2.png" alt="Custom Component " %}


```java
public class Property extends QAFWebComponent {
    @FindBy(locator = NAME_LOC)
    private QAFWebElement name;
     
    @FindBy(locator = BOOK_BTN_LOC)
    private QAFWebElement bookBtn;
     
    @FindBy(locator = RATEOPTION_COMP_LOC)
    private List<WebRateOption> rateOptions; // child component
     
    @FindBy(locator = RATEOPTION_RATECAL_COMP_LOC)
    private WebRateCalendar ratCal
    ...
    public Property(String locator) {
        super(locator);
    }
     
    // child component
    public class WebRateOption extends QAFWebComponent {
         
        @FindBy(locator = RATEOPTION_LABEL_LOC)
        private QAFWebElement label;
         
        @FindBy(locator = RATEOPTION_RATETEXT_LOC)
        private QAFWebElement rateText;
        …
        public WebRateOption(String locator) {
        super(locator);
        }
    }
}
``` 
 
```java 
public class SearchResultsPageImpl extends WebDriverBaseTestPage<WebHomePageImpl> {
     
    @FindBy(locator = CHECKINDATE_INPUT_LOC)
    private QAFWebElement checkinDate;
     
    @FindBy(locator = CHECKOUTDATE_INPUT_LOC)
    private QAFWebElement checkoutDate;
     
    @FindBy(locator = PROPERTY_COMP_LOC)
    private List<Property> properties;
     
    ...
     
    public List<Property> getProperties() {
        return properties;
    }
     
    ..
     
}
```

```	
String PROPERTY_COMP_LOC = "{'locator':'css=.property';'desc':'Property on Search Result Page'}";
String RATEOPTION_COMP_LOC = "{'locator':'css=.rateOption';'desc':'Property Rate Option'}";
String RATE_CAL_LINK_LOC = "{'locator':'css=.multiRateCalendarLink';'desc':'Browse Dates/Rates Calander Link'}";
```

{% include inline_image.html file="CustomComponent3.png" alt="Custom Component " %}

You can notice that in page list of property is defined as a list of Property component. The property component also defines RateOptions component and a calendar component.

The next step is overriding equals method to search specific component from list of component. For example below sample code demonstrate that we can match property component with string (name or id of property) or a data bean.

```java	
@Override
public boolean equals(Object obj) {
    if (obj instanceof PropertyDataBean) {
        PropertyDataBean bean = (PropertyDataBean) obj;
        return (StringUtil.isBlank(bean.getPropertyName())
                || getName().getText().trim().equalsIgnoreCase(bean.getPropertyName().trim()))
                && (StringUtil.isBlank(bean.getPropertyId())
                        || getPropertyId().trim().equalsIgnoreCase(bean.getPropertyId().trim()))
                && ((null == bean.isAvailable()) || this.isAvailable())
                && (StringUtil.isBlank(bean.getCityStateZip()) || getAddress().contains(bean.getCityStateZip()));
    }
    if (obj instanceof String) {
        return getName().getText().equalsIgnoreCase((String) obj) || getPropertyId().equalsIgnoreCase((String) obj);
    }
    return super.equals(obj);
}
```

```java	
public class PropertyDataBean extends BaseDataBean {
    private String propertyName;
    private String propertyAddress1;
    private String cityStateZip;
    private Boolean isAvailable = null;
    private String propertyId;
    private String offerName;
    private String phoneNumber;
    public PropertyDataBean() {
    }
    // getters and setters
}
```

```java
Now following code in “SearchResultsPageImpl” will return you a specific property to work with as per your requirement
private Property getProperty(Object o) {
    for (Property property : getProperties()) {
        if (property.equals(o)) {
            return property;
        }
    }
    return null;
}
```
{% include inline_image.html file="CustomComponent4.png" alt="Custom Component " %}

Above is example of reusable component in different page/component. You need to create component same as above and declare in different page/component where it is used.

```java
@FindBy(locator = CALENDAR_COMPONENT_LOC)
public WebCalendar calendar;
``` 
