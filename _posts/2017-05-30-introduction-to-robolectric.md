---
layout: post
title:  "Introduction to Robolectric"
tags: android testing unit-testing robolectric
---

This is the first in a series of articles about using [Robolectric][5] for unit testing Android code. I recently started using this powerful testing library so that I can run tests locally on my development machine. Such local unit tests do not replace instrumented tests on a device or emulator. However, they are very useful while developing individual components for an Android app. Even though Robolectric is primarily intended as a tool for Test Driven Development, I do not strictly follow the "write a test first" model for this article.

The source code for all of the apps in this series of articles can be found on [GitHub][1]. In this article, we will create a simple Android app and configure unit testing in Android Studio using Robolectric. The final version of this project is at the [`activity-test` tag][2]. See the `activity-test` subdirectory for the Android Sutdio module used in this article.

Prerequisites
==

In this article, I assume that you are already familiar with Android development and unit testing. You should be comfortable with Android Studio, the Android API (especially Activities), and JUnit 4. To review these tools, see the following links:

* [Android Studio][14]
* [Android API Training][15]
* [Android API Guides][16]
* [Android Activities][17]
* [JUnit 4][18]

[14]:https://developer.android.com/studio/index.html
[15]:https://developer.android.com/training/index.html
[16]:https://developer.android.com/guide/index.html
[17]:https://developer.android.com/guide/components/activities.html
[18]:http://junit.org/junit4/

Create an Activity
==

First we will create a simple activity to use for our first test. I will assume that you know how to create the appropriate files in Android Studio and only show the code here.

[Layout][3]
--

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/hello_text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/hello" />

</LinearLayout>
```

[Java Code][4]
--

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

Write Some Tests
==

We will use the style of JUnit 4 tests starting with something simple. The first test will create an instance of `MainActivity` and assert that it was successfully created. The final draft of all tests can be found at [`ActivityUnitTest`][8].

Add Dependency for Robolectric
--

Before we start, we need to add a dependency for Robolectric to our project. Add the following two lines to the [`dependencies`][6] block of [`build.gradle`][7]:

    testCompile 'junit:junit:4.12'
    testCompile 'org.robolectric:robolectric:3.3.2'

Create a Test Class
--

We need a class to contain our test methods. In JUnit 4, this can be any Java class. Annotations on the class and its methods tell JUnit 4 how to run the tests.

```java
@RunWith(RobolectricTestRunner.class)
@Config(constants = BuildConfig.class)
public class ActivityUnitTest {
    // Test methods go here
}
```

* All Robolectric-based tests must be run with a custom test runner provided by the Robolectric API. We tell JUnit to use `RobolectricTestRunner` with the `@RunWith` annotation.

* The `@Config` annotation tells Robolectric where to find `AndroidManifest.xml` and the Android resources in the app project. The easiest way to do this is by passing in `BuildConfig.class` as the `constants` property of the `@Config` annotation. Under the hood, Robolectric uses the values in `BuildConfig` to locate the required files.

The First Test
--

Each test is a single method annoted with `@Test`. For our first test, we will create an activity and check that the returned reference is not null.

```java
@Test
public void testActivity() {
    Activity activity = Robolectric.setupActivity(MainActivity.class);
    assertNotNull(activity);
}
```

A Second Test
--

Now let's write one more test to make sure the correct text is displayed.

```java
@Test
public void testTextView() throws Exception {
    Activity activity = Robolectric.setupActivity(MainActivity.class);
    TextView hello = (TextView) activity.findViewById(R.id.hello_text);
    assertNotNull(hello);
    assertEquals("Hello, World", hello.getText().toString());
}
```

Here we get the `TextView` with id `hello_text` in much a similar way as we would in `onCreate()`. Then we assert that the `TextView` was found and contains the expected message.

Conclusion
==

Robolectric eases the pain of testing Android apps. It allows us to write unit tests which do not have to be deployed to a device thereby decreasing the amount of time needed to run these tests.

[1]:https://github.com/codeguru42/robolectric-examples
[2]:https://github.com/codeguru42/robolectric-examples/tree/activity-test
[3]:https://github.com/codeguru42/robolectric-examples/blob/activity-test/activity-test/src/main/res/layout/activity_main.xml
[4]:https://github.com/codeguru42/robolectric-examples/blob/activity-test/activity-test/src/main/java/codeguru/robolectricexamples/MainActivity.java
[5]:http://robolectric.org/
[6]:https://github.com/codeguru42/robolectric-examples/blob/activity-test/activity-test/build.gradle#L22-L32
[7]:https://github.com/codeguru42/robolectric-examples/blob/activity-test/activity-test/build.gradle
[8]:https://github.com/codeguru42/robolectric-examples/blob/activity-test/activity-test/src/test/java/codeguru/robolectricexamples/ActivityUnitTest.java
[9]:http://hamcrest.org/JavaHamcrest/
[10]:http://junit.org/junit4/javadoc/latest/org/junit/Assert.html#assertThat(T,%20org.hamcrest.Matcher)
[11]:http://junit.org/junit4/javadoc/latest/org/junit/Assert.html#assertNotNull(java.lang.Object)
[12]:https://en.wikipedia.org/wiki/Fluent_interface
