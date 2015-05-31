---
layout: post
title: "Unit Testing Android Code using the Model-View-Presenter Pattern"
date: 2015-05-31 13:06:00 +0530
comments: true
categories: 
---
Starting Android Studio 1.1, we have [first-class unit-testing support](http://tools.android.com/tech-docs/unit-testing-support). This allows us to unit test logic in android applications without the need to start the Android device/emulator/vm.

This is big deal as the IDE now relies on local JVM to run unit-tests so that they are fast, keeping the feedback loop short. Being able to run unit-tests repeatedly and quickly allows developers to write more unit-tests and keep only minimal end-to-end functional tests (which are slow and brittle). If you'd like to know about the benefits of writing more unit-tests compared to functional tests, I'd recommend reading about the [TestPyramid](http://martinfowler.com/bliki/TestPyramid.html).

To be able to successfuly test drive logic that we write in our applications, we need to structure the code appropriately. If we end up mixing the android view/lifecycle code with the logic that we need to test, the associated JUnits will start looking more like functional (end-to-end) tests rather than unit-tests.

A popular pattern which helps move logic out of the views and facilitates testing of presentation logic is the [Model-View-Presenter](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93presenter) Patten. I've created a [video](https://www.youtube.com/watch?v=Asc4hU1iSTU) which walks through how legacy android code can be refactored using [TDD](http://en.wikipedia.org/wiki/Test-driven_development).

Code presented in the video is available on [GitHub](https://github.com/vivekjain10/AndroidUnitTesting)

* no_tests branch contains the initial code (without any unit tests)
* with_tests branch contains the the final version presented in the video
