---
layout: post
title: "Black-Box Testing - Robotium"
date: 2015-04-10 11:38:52 +0800
comments: true
categories:  [Android, Automation, Black-box Test, Robotium]
---

In previous post, it focuses on the white-box testing using Espresso.  In this post, I’m going to do Black-box test using Robotium. 
 
**What is Black-box Testing?**

Black-box testing is a method of the software testing that examines the functionality of an application without peering into its internal structure. For example, in a black-box test on Android application, the tester only knows the inputs and what the expected outcomes should be and not how the program arrives at those outputs. 

**Demonstration of the Black-box testing**

This test focuses on these three main areas: grid view, view pager and navigation drawer. The process of the testing the functionalities is exactly same as the previous [post](http://aaronliew.github.io/blog/2015/04/08/android-automation-test-espresso/).

Here is the demonstration of test:
{% youtube G9wMgmgJIfY %}

**Coding**

The code is simpler than white-box test using Espresso because it mainly focuses on the input and expected outcomes. Therefore, this test can be done without any knowledge on the codebase of targeted Android application.

{% codeblock lang:java %}
public void testGridView() throws  Exception{
        //Close the dialog and press later button
        solo.clickOnButton("Later");

        //Scroll the 3rd image of view and select it
        solo.scrollListToLine(0,3);
        solo.clickInList(0);
        solo.sleep(1000);

        //Do the swiping
        solo.scrollToSide(Solo.RIGHT);

        //Back to the main screen
        solo.goBack();

        //Scroll the 5th image of view and select it
        solo.scrollListToLine(0,5);
        solo.clickInList(0);
        solo.sleep(2000);

        //Do the swiping
        solo.scrollToSide(Solo.RIGHT);
        solo.sleep(1000);
        solo.scrollToSide(Solo.RIGHT);
        solo.sleep(1000);
        solo.scrollToSide(Solo.LEFT);

        //Back to main screen
        solo.goBack();
    }

public void testNavigationDrawer() throws  Exception{
        //Click on the later button
        solo.clickOnButton("Later");

        //Open and close the drawer
        solo.clickOnActionBarHomeButton();
        solo.sleep(2000);
        solo.clickOnActionBarHomeButton();
        solo.sleep(2000);

        //Open the drawer and select dog category
        solo.clickOnActionBarHomeButton();
        solo.sleep(2000);
        solo.clickOnText("Flickr: Dogs");
        solo.sleep(2000);

        //Open the drawer again and select tutorial
        solo.clickOnActionBarHomeButton();
        solo.sleep(2000);
        solo.drag(300, 300, 1000, 300,2);
        solo.clickOnText("Tutorial");
        solo.sleep(1500);

        //Do the swiping on tutorial screen
        solo.scrollToSide(Solo.RIGHT);
        solo.sleep(1000);
        solo.scrollToSide(Solo.RIGHT);
        solo.sleep(1000);
        solo.scrollToSide(Solo.LEFT);

        //Back to previous activity
        solo.goBack();
        solo.sleep(1500);
}

{% endcodeblock %}
