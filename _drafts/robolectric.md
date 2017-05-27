---
layout: post
title:  "Introduction to Robolectric -- Draft"
date:   2017-05-24 13:10:00 -0600
categories: android robolectric testing unit-testing
---
Introduction
--

This is the first in a series of articles about using Robolectric for unit testing Android code. I recently started using this powerful testing library so that I can run tests locally on my development machine. Such local unit tests do not replace instrumented tests on a device or emulator. However, they are very useful while developing individual components for an Android app.

The source code for all of the apps in this series of articles can be found on [GitHub][1]. In this article, we will create a simple Android app and configure unit testing in Android Studio using Robolectric. The final version of this project is at the [`activity-test` tag][2]. See the `activity-test` subdirectory for the Android Sutdio module used in this article.

Create an Activity
--

First we will create a simple activity to use for our first test. I will assume that you know how to create the appropriate files in Android Studio and only show the code here.

[Layout][3]
==

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/hello" />

</LinearLayout>
```

[Java Code][4]
==

```java
package codeguru.robolectricexamples;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

Add Dependency for Robolectric
--

Write the Test
--

[1]:https://github.com/codeguru42/robolectric-examples
[2]:https://github.com/codeguru42/robolectric-examples/tree/activity-test
[3]:https://github.com/codeguru42/robolectric-examples/blob/activity-test/activity-test/src/main/res/layout/activity_main.xml
[4]:https://github.com/codeguru42/robolectric-examples/blob/activity-test/activity-test/src/main/java/codeguru/robolectricexamples/MainActivity.java