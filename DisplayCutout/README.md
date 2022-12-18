# Problem with fullscreen app on Android phones with notch
## The essence of the problem
I must say right away I'm not a pro in flutter and android development.<br/><br/>
There is a problem on Android 9 and 10 devices with notch in full screen mode.<br/>
First you need to set the cutout mode by setting a style in your activity. 
Edit style.xml file like this:
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="AppTheme" >    
        <item name="android:windowLayoutInDisplayCutoutMode"> shortEdges </item>   <!-- most important thing --> 
        <item name="android:windowBackground">?android:colorBackground</item>
    </style>
</resources>
```
To enable fullscreen mode we need to hide status bar and navigation bar. So at the start of the app add the following line...
```dart
SystemChrome.setEnabledSystemUIMode(SystemUiMode.immersiveSticky,overlays: [] );
```
And that's our problem. The display cutout overflows our app bar. <br/><br/>
<img src="https://github.com/goliksim/Flutter/blob/main/DisplayCutout/img/1screen.jpg?raw=true"  text-align="middle" width="200" >
<br/>SafeArea does not help in this situation in any way.</br>
So, the most obvious solution to the problem is to add an insert on top yourself. But it is necessary to somehow determine the presence of the notch and its height.<br/>
In the MainActivity file.kt there is a method for determining the height of the notch using kotlin
```kt
private fun adjustToolbarMarginForNotch() {
        // Notch is only supported by >= Android 9
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.P) {
            val windowInsets = window.decorView.rootWindowInsets
            if (windowInsets != null) {
                val displayCutout = windowInsets.displayCutout
                if (displayCutout != null) {
                    val safeInsetTop = displayCutout.safeInsetTop  // safeInsetTop - our inset
                }
            }
        }
    }
```
But I haven't found a way to transfer the value to dart.
## My solution
So my solution is to determine this inset just in dart. I found only one way to do that.<br/>
We can mesure it by **systemGestureInsets** which returns an area, where your tap belongs to status bar and not to our app.</br>
Here is the discription from Google:</br> _"Simple swipe gestures that start within the systemGestureInsets areas are used by the system for page navigation and may not be delivered to the app"_</br>
```dart
stdCutoutWidth = MediaQueryData.fromWindow(window).systemGestureInsets.top;
```
But there is an another problem. Standart android hight of status bar is 24dp. So, if we use this value, then we will have an insert on the device without a notch. To solve this, we can only use values that exceed the standard Android value.
```dart
stdCutoutWidth = stdCutoutWidth>=48? stdCutoutWidth: 0;
```
I am using 48 as standart, because, according to my practice, devices with notch usually return exactly this value.</br>
We can also measure the insert for the bottom just in case for devices with both notches.
```dart
stdCutoutWidthDown = MediaQueryData.fromWindow(window).systemGestureInsets.bottom;
stdCutoutWidthDown = stdCutoutWidthDown>48? stdCutoutWidthDown/2: 0;
```
Finally add this insert as padding before scaffold.
```dart
Widget build(BuildContext context) {
    return Container(
        width: double.infinity,
        height: double.infinity,
        color: Colors.red, // your color
        padding: EdgeInsets.only(top: stdCutoutWidth*0.75,bottom: stdCutoutWidthDown*0.75),
        child:Scaffold(
          appBar: ...
          body: ...
        )
     );
}
```
## Result
<img src="https://github.com/goliksim/Flutter/blob/main/DisplayCutout/img/2screen.png?raw=true"  width="200" > <img src="https://github.com/goliksim/Flutter/blob/main/DisplayCutout/img/3screen.png?raw=true"  width="200" > <img src="https://github.com/goliksim/Flutter/blob/main/DisplayCutout/img/4screen.png?raw=true"  width="200" >
## Conclusion
I am not sure that this method is absolutely correct.</br>
**I'm also not sure if it works correctly on all devices because it doesn't have an Android 9.10 device with notch.**</br></br>
So, correct me in the comments, offer your thoughts. If this method is not suitable for your device, let me know.
