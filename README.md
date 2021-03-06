# DevAlert
This lightweight library provides visual alerts to developers and QA when an issue happens during development/testing phase.

In traditional android development, when an issue occurs, we use logs to dump the unexpected state or exception trace. Printing to logcat is not enough at times as:

-  Logs can be overlooked by developers if we are not constantly monitoring.
-  Logs maybe on a remote device which is inaccessible.

Using this library, you can provide visual warning to the developer/ QA when something goes wrong on your test or internal builds so that critical issues can be highlighted as and when they happen.

<p align="center">
    <img src="http://i.imgur.com/lPTL6op.gif" width="300"/>
</p>

## Example
In the example below, the `ParseException` may occur only for some specific response from server and is not always reproducible.

```java
try {
    Data data = parser.parse(response.body().json());
} catch(ParseException ex) {
    Log.d(TAG, ex);
    DevAlert.reportError(TAG, "Response data is corrupt", ex);
}
```

While development or testing, when the issue happens, even if the developer/QA is not actively looking or monitoring the log for this issue, will get instantly notified.


## How to Use
Add the dependency in `build.gradle`:

```
dependencies {
    ...
    ...
    compile 'com.garena.devalert:dev-alert:0.1.2'
}
```

Initialise the library in the `Application` class:

```java
public class SampleApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        DevAlert.init(this, BuildConfig.DEBUG);
    }
}
```

Report errors or warning from anywhere in the app:

```java
// report error
DevAlert.reportError(TAG, message, exception);

// report warning
DevAlert.reportWarning(TAG, message, exception);
```

## Other Possible Use-Cases


1. Report exceptions from background threads without crashing the app.

2. If you are building a library or SDK, you can provide better feedback to other developers who are going to use it. (If you have worked with React-Native, it provides a similar functionality.)

3. Create custom rules for the health check of your app and visually alert the developer/QA when these rules are violated. For example: database calls on UI thread or adding heavy code in onCreate.

## Advance Usage
You can configure the library to show selective errors or warnings use the `DevAlertConfig:`

```java
DevAlertConfig config = new DevAlertConfig.Builder()
      .showErrors(true)
      .showWarnings(false) // hide warnings
      .ignoredTags(Arrays.asList("MODULE_A", "MODULE_B")) // hide these tags
      .build();
DevAlert.init(getApplication(), isDevMode, config);
```

## More Info

Here is an informal blog post https://engineering.garena.com/introducing-devalert-garena-open-source/ which lists some scenarios in which we have used this library to improve our product.