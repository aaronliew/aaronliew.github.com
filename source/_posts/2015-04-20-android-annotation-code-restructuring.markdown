---
layout: post
title: "Android Annotation"
date: 2015-04-20 10:18:17 +0800
comments: true
categories: [Android Annotations, Android Development]
---

Android Annotation(AA) is framework that facilitate the writing and the maintenance of Android Applications, and greatly simplifies the code. It generates subclasses at compile time, for example, the subclass of the MainActivity class would be MainActivity_. To understand how the AA works, you can have a look at the subclass of the Activity. 

**How to enhance Android Code with AA**

I would like to discuss the common annotations that would be used in Android Development. 

Add @EActivity with layout id at the top of the class to replace setContentView of the Activity Class

{% codeblock lang:java %}
@EActivity(R.layout.sliding_menu)
public class BaseActivity extends ActionBarActivity
{% endcodeblock %}

Add @ViewById to replace findViewById
{% codeblock lang:java %}
@ViewById(R.id.drawer_layout)
DrawerLayout mDrawerLayout;
{% endcodeblock %}

Add @Bean to inject the activity context into class
{% codeblock lang:java %}
@Bean
GlobalApplication globalApplication;
{% endcodeblock %}

Add @AfterView before method to do something after view injection, for example, setText, setBackground, etc.. 
{% codeblock lang:java %}
@AfterViews
public void initLayout()
{% endcodeblock %}

Add @Click method with view's id to handle OnClickListener. 
{% codeblock lang:java %}
@Click(R.id.about_us_linearLayout)
public void ClickAboutUs()
{% endcodeblock %}
That's it. There is no need to do "findViewById" to get the view then followed by setOnClickListener.

Want to run a thread? Add @Background above the method 
{% codeblock lang:java %}
@Background
public void DownloadAndSetImage()
{% endcodeblock %}

Initiate the Layoutinflator using @SystemService
{% codeblock lang:java %}
@SystemService
LayoutInflater inflator;
{% endcodeblock %}

Add EBean at the top of the CLass in order to use available annotations
{% codeblock lang:java %}
@EBean
public class GlobalApplication
{% endcodeblock %}

Add @RootContext to inject the context into the class 
{% codeblock lang:java %}
@RootContext
static Context context;
{% endcodeblock %}









