---
layout: post
title: "RxAndroid"
date: 2015-04-15 11:42:08 +0800
comments: true
categories: [RxAndroid, RxJava, Reactive, Java]
---

**What is Reactive Java?**

Reactive Java is the library that manages sequences of event by using observable sequences. It is currently implemented by Netflix. Here is the demonstration of the RxJava:

{% img center /images/RxJava/RxExample.png Example of RxJava %}

In this example, circular item represents an event where event could be click event, caches, variables, data. It ensures that the event is completed before proceed 
to the next event. Each events can be modified into new form before proceed to the next action. For example,  It applies "Map" function to the observable sequences into new form.

{% img center /images/RxJava/Map.png Map %}

*Basic Explanation on Observable*

{% img center /images/RxJava/Explanation.png Map %}

**Common Usage of RxAndroid**

*Lifecycle observable*

RxAndroid is able to create a subscription that is bound to lifecycle: OnStart, OnPause and OnResume. It subscribes to an observable on "OnStart" and unsubscribes observable on "onPause" and "onDestroy" lifecycle.  It is important to do so to avoid NullPointerException. NullPointerException happens if it tries to update the UI which is already destroyed after stream of events is completed. 

*Listening observable*

Whenever there is rotation of screen, the data and activity will be destroyed. RxJava provides feature that always listening to the background sequence which keeps emitting items, and updating UI components, for example Textviews even after screen rotation or detached from the screen.

*Implementation of Lifecycle and Listening observable*

In OnCreate, start the sequences. 
{% codeblock lang:java %}
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    strings = SampleObservables.numberStrings(1, 50, 250).publish();
    strings.connect(); // trigger the sequence
}
{% endcodeblock %}

In OnViewCreated of the Fragment, do the subscription. It means whenever the view is created, it will subscribe to an observable and continue to update the UI components i.e. TextView.

{% codeblock lang:java %}
subscription = AppObservable.bindFragment(this, strings).subscribe(new Subscriber<String>() {
	
	@Override
	public void onCompleted() {
		Toast.makeText(getActivity(), "Done!", Toast.LENGTH_SHORT).show();
	}

	@Override
	public void onError(Throwable throwable) {

	}

	@Override
	public void onNext(String s) {
		textView.setText(s);
	}
});
{% endcodeblock %}

Unsubscribe the observable whenever the view is destroyed/detached due to screen rotation to prevent NPE.

{% codeblock lang:java %}

@Override
public void onDestroyView() {
    subscription.unsubscribe();
    super.onDestroyView();
}

{% endcodeblock %}


The following code is to bound the subscription to lifecycle.
{% codeblock lang:java %}

@Override
protected void onPause() {
    super.onPause();

    // Should output "false"
    Log.i(TAG, "onPause(), isUnsubscribed=" + subscription.isUnsubscribed());
}

@Override
protected void onStop() {
    super.onStop();

    // Should output "true"
    Log.i(TAG, "onStop(), isUnsubscribed=" + subscription.isUnsubscribed());
}

{% endcodeblock %}

**Reference**

1. https://gist.github.com/staltz/868e7e9bc2a7b8c1f754
2. http://reactivex.io/documentation/operators/map.html
3. http://reactivex.io/RxJava/javadoc/rx/Observable.html#subscribe(rx.Subscriber)


