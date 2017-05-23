Put testing how-to guides, presentations, and other info here.

## **General notes on testing in Android**

For much of the history of the Android platform the only testing strategy available for local development was system testing - i.e.: running tests inside of a live Dalvik VM running on a device or emulator. 

Thankfully, with the arrival of Android Studio and gradle build plugin v1.1 welcome changes finally ushered in Android unit testing support -  i.e.: support and tooling that enables the development and execution of unit tests within the local IDE. In addition to these changes the build plugin also added the `mockableAndroidJar` task, which strips away the `final` modifier from Android components allowing them to be stubbed/proxied/mocked in your tests!

### **Testing flavors/strategies:**

In a nutshell there are currently 3 core types of testing tools/environments available to the developer: 

* (Dalvik VM, Device-based) Natively-instrumented testing frameworks such as Espresso or the legacy Android Instrumentation framework.
  
  
* (JVM based) "Shadow" frameworks like Robolectric, which provide the speed benefit of running in a JVM while also providing non-device-based instrumentation/testing of Android applications.
  
  
* (JVM based) Classic stand-alone unit/integration testing.
  
  

**Trade-offs:**

* Device-based tests provide full testing support and generally provide the highest confidence level due to the fact that they rely solely on native code, instrumentation, and runtime. However they also require the most overhead and are the slowest to run and develop/maintain.
  
  
* Shadow frameworks provide huge gains in dev/run times, however they provide incomplete testing support and a lower confidence level than native testing environments.
  
  
* Classic Unit testing environments provide the fastest dev/run times but they don't include any Android-specific testing support.

### BEST PRACTICES:

(See [Android Team Unit Testing Audit](https://docs.google.com/a/getvictorious.com/presentation/d/1WDz7zK0oSvtFUGuX9gy-psQwdZ3URitZftejttfx-9E/edit?usp=sharing) for an overview of testing issues, best practices, gotchyas, and go-forward (as of 03/2016) recommendations)[](https://docs.google.com/a/getvictorious.com/presentation/d/1WDz7zK0oSvtFUGuX9gy-psQwdZ3URitZftejttfx-9E/edit?usp=sharing)

* Use a "tiered" approach to testing. Always use the "lightest" solution available. 
  For example, if code and related testing goals can truly be isolated to business logic, outside of Android system and UI concerns, they should be written or re-written as "vanilla" JUnit tests instead of a Robolectric or Espresso tests. Doing this will provide gains in both dev and build time, and make the related tests more portable as well.
  
  
* For Android-specific testing consider using native test coverage for application releases, while favoring "Shadow"-based test coverage for development.
  "Shadow"-based testing should provide enough speed and ease of development to realistically support the option of employing a TDD dev approach (using Robolectric), while native testing will satisfy the confidence level required for release/delivery (using Espresso).
   

## **Definitions**

*Collaborator*- any objects that interact with  O.U.T. during test exec

*Instrumented unit tests** -*** unit tests that run on physical devices and emulators, instead of the Java Virtual Machine (JVM) on your local machine.

*mock* - a object where most methods will have null/false/zero

*OUT*- object under test

*stub*- a mock object where some methods will return useful data

 

# Tools

## **Mockito** - Mock Objects, stubbing, argumentCaptors

Slides by Bret [https://docs.google.com/presentation/d/1CqAxLIIoHdGwd1WFUCXcQRtm1GKexLQ9RZIVCJT9_D8/](https://docs.google.com/presentation/d/1CqAxLIIoHdGwd1WFUCXcQRtm1GKexLQ9RZIVCJT9_D8/)

## **Espresso** - UI Testing

Slides by Edward [https://docs.google.com/presentation/d/1EkvcYz8lU8u-uAnO2A-o_-FQXro2aK-EA6Ltt-cAJBI/](https://docs.google.com/presentation/d/1EkvcYz8lU8u-uAnO2A-o_-FQXro2aK-EA6Ltt-cAJBI/)

Cheat sheet  [https://google.github.io/android-testing-support-library/docs/espresso/cheatsheet/](https://google.github.io/android-testing-support-library/docs/espresso/cheatsheet/)

**Testing Environment:**

* Androideka / staging

**How to run tests:**

* From terminal: ./gradlew cAT or ./gradlew connectedAndroid

**How to run specific test:**

* From Android Studio: Click the test file -> Run -> Run -> select test file
* From Terminal: ./gradlew -Pandroid.testInstrumentationRunnerArguments.class=your.test.file.package.TestClass connectedAndroid

## **Robolectric - **JVM-based, "shadowed" Android system/UI testing

Cheat sheet (somewhat dated but works) [https://github.com/danialgoodwin/dev/blob/master/android/testing/robolectric-cheat-sheet.md](https://github.com/danialgoodwin/dev/blob/master/android/testing/robolectric-cheat-sheet.md)

Set up:

gradle provides separate classpath configuration for tests that use native Android Instrumentation (e.g.: native Android framework and Espresso) vs. standalone tests that run in the JVM (respectively `androidTestCompile` and  `testCompile`). Specify the necessary dependencies with `testCompile` declarations:
```
// Robolectric
testCompile 'junit:junit:4.12'
testCompile 'org.mockito:mockito-core:1.10.19'
testCompile 'org.hamcrest:hamcrest-core:1.1'
testCompile 'org.hamcrest:hamcrest-library:1.1'
testCompile 'org.hamcrest:hamcrest-integration:1.1'
testCompile "org.robolectric:robolectric:3.0"
testCompile "org.robolectric:shadows-multidex:3.0"
testCompile "org.robolectric:shadows-support-v4:3.0"
```
    2. Place your test class under src/test/java

    3. Important: when writing or running Robolectric tests (or any other standalone tests that live under src/test) you MUST use the Android Studio "Build Variants" widget and toggle the "Test Artifact" setting to "Unit Tests"

    4. Have your test class extend RoboConf and annotate the class declaration with the robolectric runner:


```
@RunWith(RobolectricGradleTestRunner.class)
public class ModelTest extends RoboConf {
```
   5. Use the following guides to help you get started writing tests using the Robolectric framework and APIs, and/or check out existing Robolectric tests (as they continue to develop/appear in our code base)

[http://robolectric.org/](http://robolectric.org/)

[http://robolectric.org/getting-started/](http://robolectric.org/getting-started/)

[http://robolectric.org/writing-a-test/](http://robolectric.org/writing-a-test/)

[http://robolectric.org/javadoc/3.0/index.html](http://robolectric.org/javadoc/3.0/index.html)

 

## TROUBLESHOOTING

java.lang.RuntimeException: java.lang.RuntimeException: build/intermediates/bundles/debug/AndroidManifest.xml not found or not a file; it should point to your project's AndroidManifest.xml*
  *[https://github.com/robolectric/robolectric/issues/1648
  ](https://github.com/robolectric/robolectric/issues/1648)Open Edit Configuration

![](wiki-attachment:Screen Shot 2016-01-22 at 11.19.13 AM.png)

Set DEFAULT Working Directory to $MODULE_DIR$  (you might also need to do so for existing tests)![](wiki-attachment:Screen Shot 2016-01-22 at 2.56.07 PM.png)

 

 

 

