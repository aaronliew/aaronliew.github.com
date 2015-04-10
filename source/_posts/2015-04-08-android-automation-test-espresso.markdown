---
layout: post
title: "White-Box Testing- Espresso"
date: 2015-04-08 01:13:17 +0800
comments: true
categories: [Android, Automation, White-box Test, Espresso]
---

One common approach to UI testing is to run tests manually and verify that the app is behaving as expected. However, this approach can be very time-consuming, tedious, and error-prone especially when we run tests on multiple devices. This is where Android Automated Tests becomes useful, it can perform repetitive task without human intervention. 

**What is White-box Testing?**

There are two types tests: white-box test and black-box test. White-box test is method of testing software that tests internal structures or workings of an application. Black-box test is method of examining the functionality of an application without peering into its internal structures or workings. In this project, I did white-box test on my code using Espresso library.

**Advantage of Espresso**

The advantage of the Espresso is it is have synchronization feature, meaning it waits for UI events in the current message queue to process and default task to complete before it moves on to the next test operation.

**Demonstration of the Espresso**

I have written test script on animal gallery app. Here are the following steps to verify the functionality of the app:

<ol>
<li>Launch Animal Gallery app</li>
<li>“Donation” screen will be shown up. Press “Later” button to proceed to main screen</li>
</ol>

***Test on functionality of the grid view***

<ol start="3">
  <li>Scrolls to the 3rd image and tap on it. </li>
  <li>In full screen viewpager, swipe to the left and view the next image.</li>
  <li>Back to the main screen</li>
  <li>Scrolls to the 6th image and tap on it. </li>
  <li>In full screen viewpager, swipe to the left twice and view the image.</li>
  <li>Swipe to the right again to go back the previous image.</li>
</ol>

***Test on functionality of the navigation drawer***

<ol start="9">
  <li>Tab on the navigation drawer icon to open the drawer</li>
  <li>Search for “Flickr: Dogs” category and tab on it.</li>
  <li>Open the drawer again and select “Tutorial” category.</li>
  <li>Swipes the tutorial screen.</li>
  <li>Done.</li>
</ol>


Here is the demonstration of the test:

{% youtube G9wMgmgJIfY %}


**Coding**

{% codeblock lang:java %}
 public void testGridView() {
        //Close the dialog and press later button
        onView(withId(R.id.later_button))
        .perform(click());
        onView(isRoot()).perform(waitAtLeast(2000));

        //Scroll the 3rd image of view and select it
        onData(anything())
        .inAdapterView(allOf(withId(R.id.gridview), isDisplayed()))
        .atPosition(2)
        .check(matches(isDisplayed()))
        .perform(click());
        onView(isRoot()).perform(waitAtLeast(2000));

        //Do the swiping 
        onView(withId(R.id.pager))
          .perform(swipeLeft());
            
        //Back to the main screen
        Espresso.pressBack();

        //Scroll the 5th image of view and select it
        onData(anything())
                .inAdapterView(allOf(withId(R.id.gridview), isDisplayed()))
                .atPosition(5)
                .check(matches(isDisplayed()))
                .perform(click());
        onView(isRoot()).perform(waitAtLeast(2000));
        
        //Do the swiping
        onView(withId(R.id.pager))
                .perform(swipeLeft());
        onView(withId(R.id.pager))
                .perform(swipeLeft());
        onView(withId(R.id.pager))
                .perform(swipeRight());
        
        //Back to main screen
        Espresso.pressBack();


}           


public void testNavDrawer(){

        //Click on the later button
        onView(withId(R.id.later_button))
                .perform(click());

        //Open and close the drawer
        onView(withContentDescription(getActivity().getString(R.string.drawer_open)))
			.perform(click());
        onView(withContentDescription(getActivity().getString(R.string.drawer_close)))
			.perform(click());

        //Open the drawer and select dog category
        onView(withContentDescription(getActivity().getString(R.string.drawer_open)))
			.perform(click());
        onView(Matchers.allOf(ViewMatchers.withId(R.id.sm_item_title), hasSibling(ViewMatchers.withText("Flickr: Dogs")))).perform(click());

        //Open the drawer again and select tutorial
        onView(withContentDescription(getActivity().getString(R.string.drawer_open)))
			 .perform(click());
        onView(isRoot()).perform(waitAtLeast(2000));
        onView(withId( R.id.tutorial)).perform( scrollTo(), click());

        //Do the swiping on tutorial screen
        onView(withId(R.id.tutorial_pager))
        .perform(swipeLeft());
        onView(withId(R.id.tutorial_pager))
                .perform(swipeLeft());
        onView(withId(R.id.tutorial_pager))
                .perform(swipeRight());

        //Back to previous activity
        Espresso.pressBack();
        onView(isRoot()).perform(waitAtLeast(2000));

}

{% endcodeblock %}







