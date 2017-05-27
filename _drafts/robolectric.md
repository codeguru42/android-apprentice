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

Add Dependency for Robolectric
--

Write the Test
--

[1]:https://github.com/codeguru42/robolectric-examples
[2]:https://github.com/codeguru42/robolectric-examples/tree/activity-test
