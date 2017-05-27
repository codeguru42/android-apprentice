---
layout: post
title:  "Introduction to Robolectric"
tags: android testing unit-testing robolectric
---
Introduction
==

This is the first in a series of articles about using [Robolectric][5] for unit testing Android code. I recently started using this powerful testing library so that I can run tests locally on my development machine. Such local unit tests do not replace instrumented tests on a device or emulator. However, they are very useful while developing individual components for an Android app.

The source code for all of the apps in this series of articles can be found on [GitHub][1]. In this article, we will create a simple Android app and configure unit testing in Android Studio using Robolectric. The final version of this project is at the [`activity-test` tag][2]. See the `activity-test` subdirectory for the Android Sutdio module used in this article.

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

Add Dependency for Robolectric
==

Now we need to add a dependency for Robolectric to our project. Add the following two lines to the [`dependencies`][6] block of [`build.gradle`][7]:

    testCompile 'junit:junit:4.12'
    testCompile 'org.robolectric:robolectric:3.3.2'

Write the Test
==

Finally, we can start writing tests. We will use the style of JUnit 4 tests starting with something simple. The first test will create an instance of `MainActivity` and assert that it was successfully created. Here is the code for [`ActivityUnitTest`][8]:

```java
package codeguru.robolectricexamples;

import android.app.Activity;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.robolectric.Robolectric;
import org.robolectric.RobolectricTestRunner;
import org.robolectric.annotation.Config;

import static org.hamcrest.core.IsNull.notNullValue;
import static org.junit.Assert.assertThat;

@RunWith(RobolectricTestRunner.class)
@Config(constants = BuildConfig.class)
public class ActivityUnitTest {
    @Test
    public void testActivity() {
        Activity activity = Robolectric.setupActivity(MainActivity.class);
        assertThat(activity, notNullValue());
    }
}
```

Breakdown
--

1. All Robolectric-based tests must be run with a custom test runner provided by the Robolectric API. We tell JUnit to use `RobolectricTestRunner` with the `@RunWith` annotation.

2. The `@Config` annotation tells Robolectric where to find `AndroidManifest.xml` and the Android resources in the app project. The easiest way to do this is by passing in `BuildConfig.class` as the `constants` property of the `@Config` annotation. Under the hood, Robolectric uses the values in `BuildConfig` to locate the required files.

3. We write the test in a method annotated with `@Test`. First, we call `Robolectric.setupActivity(MainActivity.class)` to create the activity. Then we assert that the returned reference is not null. I decided to use [`assertThat()`][10] which uses a [Hamcrest matcher][9]. Alternatively, you can use [`assertNotNull()`][11]. Using Hamcrest matchers provides a [fluent syntax][12] which is much easer to read and write than the traditional JUnit assertions.

A Second Test
==

Let's write one more test to make sure the correct text is displayed.

```java
@Test
public void testTextView() throws Exception {
    Activity activity = Robolectric.setupActivity(MainActivity.class);
    TextView hello = (TextView) activity.findViewById(R.id.hello_text);
    assertNotNull(hello);
    assertEquals("Hello, World", hello.getText().toString());
}
```

Here we get the `TextView` with id `hello_text` and assert that it contains the expected message.

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
