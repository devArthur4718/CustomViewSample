# CustomViewSample
Creating custom view with canvas , paint and overriding on click method in order to respond to user interaction my moving the dial from position to position

![](https://i.imgur.com/7k9pdkh.png)

# Steps in Creating a Custom view. 

Any view in Android can be customizd. In order to do that, you can inherit directly from the view class or from a child class  
like image Views. Since we are using Kotlin that runs along with the JVM, an annotation is needed in order to inform the compiler
that this classes atributes needs to override the default parameter values from the View. 


```kotlin
class DialView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : View(context, attrs, defStyleAttr)
```

# Project details 

* Android Studio Version 3.6.1 
* Gradle Version 5.6.4 

# Project reference.

https://developer.android.com/reference/android/app/job/JobScheduler 
https://codelabs.developers.google.com/codelabs/android-training-job-scheduler/index.html#2
