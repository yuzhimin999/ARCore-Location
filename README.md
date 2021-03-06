![Example Annotation](http://smegaupload.co.uk/up/uploads/arcorelocationbanner1524843962.png "ARCore location gps")

# ARCoreLocation

[Allows items to be placed within the AR world using real-world coordinates.](https://www.appoly.co.uk/arcore-location/)

Built for the ARCore Android SDK.

## Example usage
It's first required to set-up a basic ARCore project, such as [Google's Hello AR example](https://github.com/google-ar/arcore-android-sdk/tree/master/samples/hello_ar_java).

### Importing the library
Add the JitPack repository to your build file

#### Step 1. 
Add JitPack in your root build.gradle at the end of repositories:
```
allprojects {
	repositories {
		...
		maven { url 'https://jitpack.io' }
	}
}
```

#### Step 2. 
Add the ARCore-Location dependency. Replace `0.1.0` with the latest release from the [releases tab on Github](https://github.com/appoly/ARCore-Location/releases)
```
dependencies {
    compile 'com.github.appoly:ARCore-Location:0.1.0'
}
```

### Using the library
We've included a couple of example renderers (to render something at a particular location). These are `AnnotationRenderer` and `ImageRenderer`. An example of adding a custom renderer is in /examples/hello_ar_example with `ObjectRenderer`.

To implement this library into one of your AR projects, do the following.

#### Step 1. 
Inside your AR Activity, you should create a new variable called `locationScene`, which will be the instance of this library.
```
private LocationScene locationScene;
```


#### Step 2.
Annotations linked to GPS coordinates can be added in the `onCreate` method.
```
// Annotation at Buckingham Palace
// Shows toast on touch
LocationMarker buckinghamPalace =  new LocationMarker(
    0.1419,
    51.5014,
    new AnnotationRenderer("Buckingham Palace")
);
buckinghamPalace.setOnTouchListener(new Runnable() {
    @Override
    public void run() {
        Toast.makeText(HelloArActivity.this,
                "Touched Buckingham Palace", Toast.LENGTH_SHORT).show();
    }
});
locationScene.mLocationMarkers.add(buckinghamPalace);
```


Images can similarly be added like so
```
// Image marker at Eiffel Tower
locationScene.mLocationMarkers.add(
    new LocationMarker(
        2.2945,
        48.858222,
        new ImageRenderer("eiffel.jpg")
    )
);
```

#### Step 3. 
You must call `locationScene.resume();` within your Activity's `onResume()` method, and similarly call `locationScene.pause();` within your `onPause()` method.

#### Step 4. 
For the library to draw your annotations and images, you must add `locationScene.draw(frame);` to the `onDrawFrame(GL10 gl)` method as so:
```
// Draw background.
mBackgroundRenderer.draw(frame);

// Draw location markers
locationScene.draw(frame);
```

#### Permissions
This library requires permission to use the device Camera and Fine Location. You should set this up in `AndroidManifest.xml`. If you're unfamiliar with requesting permissions, have a look at HelloArActivity in our example project.
```
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

### Support
If you're having problems with the library, please [open a new issue](https://github.com/appoly/ARCore-Location/issues), and we'll aim to address it quickly.

#### Known issues
Mobile phone compasses only have an accuracy of about 15 degrees, even when calibrated. For most applications this is adequate, but when trying to superimpose markers over the real world it can be very problematic. This is a problem we haven’t resolved, and welcome your ideas on how best to do so!

### Contributing
We'd love your help in making this library better. Pull requests with new features and bug fixes are welcome.

### Apps built with ARCore-Location
[Where's my cAR?](https://play.google.com/store/apps/details?id=uk.co.appoly.wheres_my_car) - Appoly