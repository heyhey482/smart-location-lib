smart-location-lib
==================

Android library project that intends to use the minimum battery drain possible when used in navigation apps.

Getting started
---------------

You should add this dependency.

````xml
<dependency>
	<groupId>com.mobivery.smartlocation</groupId>
	<artifactId>library</artifactId>
	<version>1.0-SNAPSHOT</version>
</dependency>
````

Permissions
-----------

You must add these permissions to your AndroidManifest.xml. 

````xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-permission android:name="com.google.android.gms.permission.ACTIVITY_RECOGNITION"/>
<uses-permission android:name="com.google.android.providers.gsf.permission.READ_GSERVICES"/>

````

You also have to add two services to your application node in the manifest.

````xml
<service android:name="com.mobivery.smartlocation.ActivityRecognitionService"/>
<service android:name="com.mobivery.smartlocation.SmartLocationService"/>
````

Check out the sample project for seeing how it should be in a real situation.

Usage
-----

For starting the location service, you must perform just one call.

````java
SmartLocation.getInstance().start(context);
````

For stopping the location (but with a chance to restarting it) you should use the stop method.

````java
SmartLocation.getInstance().stop(context);
````

For stopping it for good and avoid service leaks, you must call the cleanup method this way.

````java
SmartLocation.getInstance().cleanup(context);
````

The Service will send the information of user's location and current activity via intents.

The default intent that will be broadcasted will be `com.mobivery.smartlocation.LOCATION_UPDATED`, but you can configure the package by using a SmartLocationOptions object but more on that later.

Here goes an example of how to capture the intent and do things with it.

````java
    private void captureIntent() {
        IntentFilter locationUpdatesIntentFilter = new IntentFilter(LOCATION_UPDATED_INTENT);
        registerReceiver(locationUpdatesReceiver, locationUpdatesIntentFilter);
    }

    private void releaseIntent() {
        unregisterReceiver(locationUpdatesReceiver);
    }

    private BroadcastReceiver locationUpdatesReceiver = new BroadcastReceiver() {
        @Override
        public void onReceive(Context context, Intent intent) {
            Location location = intent.getParcelableExtra(SmartLocation.DETECTED_LOCATION_KEY);
            int activity = intent.getIntExtra(SmartLocation.DETECTED_ACTIVITY_KEY, DetectedActivity.UNKNOWN);
            showLocation(location, activity);
        }
    };

    private void showLocation(Location location, int activityType) {
        // Do your thing here!
    }
````

Call the captureIntent method when you want to receive your updates, and call the releaseIntent when you want to stop receiving them. 

Customizing to your needs
-------------------------
In the following example you can see how to setup a new package for the LOCATION_UPDATED intent.

````java
        SmartLocationOptions options = new SmartLocationOptions();
        options.setPackageName("com.mypackage.name");
        SmartLocation.getInstance().start(context, options);
````

With this call, the intent you will want to watch for is `com.mypackage.name.LOCATION_UPDATED`.

License
-------

The MIT License (MIT)

Copyright (c) 2013 Mobivery

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
