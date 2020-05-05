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
Following up, we just need to use Paint and Canvas to color and draw or custom view. 

```kotlin
private var radius = 0.0f                   // Radius of the circle.
    private var fanSpeed = FanSpeed.OFF         // The active selection.
    // position variable which will be used to draw label and indicator circle position
    private val pointPosition: PointF = PointF(0.0f, 0.0f)

    init {
        isClickable = true
    }
    //Initial view state.
    private val paint = Paint(Paint.ANTI_ALIAS_FLAG).apply {
        style = Paint.Style.FILL
        textAlign = Paint.Align.CENTER
        textSize = 55.0f
        typeface = Typeface.create("", Typeface.BOLD)
    }

    //Called anytime view size changes.
    override fun onSizeChanged(w: Int, h: Int, oldw: Int, oldh: Int) {
        radius = (min(width,height) / 2.0 * 0.8).toFloat()
    }

    //Calculate a point using radius as origin
    private fun PointF.computeXYForSpeed(pos: FanSpeed, radius : Float){
        //Angles are in radians.
        val startAngle = Math.PI * ( 9 / 8.0)
        val angle = startAngle + pos.ordinal * (Math.PI / 4)
        x = (radius * cos(angle)).toFloat() + width / 2
        y = (radius * sin(angle)).toFloat() + height / 2
    }

    override fun onDraw(canvas: Canvas?) {
        super.onDraw(canvas)

        //Draw the big circle
        paint.color = if(fanSpeed == FanSpeed.OFF) Color.GRAY else Color.GREEN
        canvas?.drawCircle((width / 2).toFloat(), (height / 2).toFloat(),radius, paint)

        //Draw small position indicators.
        val markerRadius = radius + RADIUS_OFFSET_INDICATOR
        pointPosition.computeXYForSpeed(fanSpeed,markerRadius)
        paint.color = Color.BLACK
        canvas?.drawCircle(pointPosition.x, pointPosition.y, radius/12, paint)

        //Draw text labels
        val labelRadius = radius + RADIUS_OFFSET_LABEL
        for( i in FanSpeed.values()){
            pointPosition.computeXYForSpeed(i, labelRadius)
            val label = resources.getString(i.label)
            canvas?.drawText(label, pointPosition.x, pointPosition.y, paint)


        }

    }

    override fun performClick(): Boolean {
        if(super.performClick()) return true

        fanSpeed = fanSpeed.next()
        contentDescription = resources.getString(fanSpeed.label)

        invalidate() //Draw view again
        return true
    }
```

# Project details 

* Android Studio Version 3.6.1 
* Gradle Version 5.6.4 

# Project reference.

https://developer.android.com/reference/android/app/job/JobScheduler 
https://codelabs.developers.google.com/codelabs/android-training-job-scheduler/index.html#2
