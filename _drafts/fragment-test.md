---
layout: post
title:  "Testing Fragments with Robolectric"
tags: android testing unit-testing robolectric
---

In this article, we will learn how to write unit tests for a `Fragment` subclass. This is a continuation of a series of articles about testing with [Robolectric][1]. If you want to learn about the basics of testing with Robolectric, read the [first article][9] in this series.

The source code for all of the apps in this series can be found on [GitHub][2]. The final version of the Android Studio module used in this article is in the `fragment-test` subdirectory at the [`fragment-test` tag][3].

Prerequisites
==

In this article, I assume that you are already familiar with Android development and unit testing. You should be comfortable with Android Studio, the Android API (especially Activities and Fragments), and JUnit 4. To review these tools, see the following links:

* [Android Studio][4]
* [Android API Training][5]
* [Android API Guides][6]
* [Android Activities][7]
* [JUnit 4][8]

Create the App to Test
==

The app we test in this article is very similar to the one tested in the [previous article][9] in this series. The difference is that we will use a `Fragment` to contain the UI components. As before, I assume you know how to create the appropriate classes and XML layout files in Android Studio. Here is the code:

[Java Code For `MainActivity`][10]
--

```java
package codeguru.fragmenttest;

import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

}
```

[XML layout for `MainActivity`][11]
--

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/fragment"
    android:name="codeguru.fragmenttest.MainFragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layout_behavior="@string/appbar_scrolling_view_behavior"
    tools:layout="@layout/fragment_main" />
```

[Java Code for `MainFragment`][12]
--

```java
package codeguru.fragmenttest;

import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

public class MainFragment extends Fragment {

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_main, container, false);
    }

}
```

[XML layout for `MainFragment`][13]
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

Write the Tests
==

Conclusion
==

[1]:http://robolectric.org/
[2]:https://github.com/codeguru42/robolectric-examples
[3]:https://github.com/codeguru42/robolectric-examples/tree/fragment-test
[4]:https://developer.android.com/studio/index.html
[5]:https://developer.android.com/training/index.html
[6]:https://developer.android.com/guide/index.html
[7]:https://developer.android.com/guide/components/activities.html
[8]:http://junit.org/junit4/
[9]:http://androidapprentice.com/2017/05/30/introduction-to-robolectric.html
[10]:https://github.com/codeguru42/robolectric-examples/blob/fragment-test/fragment-test/src/main/java/codeguru/fragmenttest/MainActivity.java
[11]:https://github.com/codeguru42/robolectric-examples/blob/fragment-test/fragment-test/src/main/res/layout/activity_main.xml
[12]:https://github.com/codeguru42/robolectric-examples/blob/fragment-test/fragment-test/src/main/java/codeguru/fragmenttest/MainFragment.java
[13]:https://github.com/codeguru42/robolectric-examples/blob/fragment-test/fragment-test/src/main/res/layout/fragment_main.xml
