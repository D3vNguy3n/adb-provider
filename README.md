# ADB Provider

Android ADB pairing and shell library published on Maven Central.

## Gradle

Make sure the project uses Maven Central in `settings.gradle`:

```gradle
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
    }
}
```

Add the dependency to the app module:

```gradle
dependencies {
    implementation "io.github.d3vnguy3n:adb-provider:1.0.8"
}
```

## Required AndroidManifest.xml entries

Version 1.0.8 leaves all permissions, the provider, and the service under
host-app control. Add these before the app's `<application>` element:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.CHANGE_WIFI_MULTICAST_STATE" />
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
<uses-permission
    android:name="android.permission.NEARBY_WIFI_DEVICES"
    android:usesPermissionFlags="neverForLocation" />
<uses-permission android:name="android.permission.USE_LOOPBACK_INTERFACE" />
<uses-permission android:name="android.permission.ACCESS_LOCAL_NETWORK" />
```

Add these inside `<application>`:

```xml
<meta-data
    android:name="com.dnturbo.adb.KEY_NAME"
    android:value="Any device name" />

<provider
    android:name="com.dnturbo.adb.AdbProvider"
    android:authorities="${applicationId}.com.dnturbo.adb.provider"
    android:exported="false"
    android:initOrder="100" />

<service
    android:name="com.dnturbo.adb.AdbPairingService"
    android:exported="false" />
```

## Initialize once

```java
import android.app.Application;
import com.dnturbo.adb.AdbShell;

public final class App extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        AdbShell.initialize(this, "My app");
    }
}
```

Use the same value for `com.dnturbo.adb.KEY_NAME` in the manifest. The bundled
`AdbProvider` also initializes the library automatically when the process starts.

## Connect

Call this from an `Activity`, for example from a Connect button. The library first
tries the saved RSA key. If that is unavailable, it opens Wireless debugging and
shows the notification used to enter Android's six-digit pairing code.

```java
AdbShell.get().connect(this, new AdbShell.ConnectionCallback() {
    @Override
    public void onStateChanged(AdbShell.ConnectionState state) {
        // CHECKING_SETTINGS, TRYING_RSA, WAITING_FOR_PAIRING_CODE, PAIRING, ...
    }

    @Override
    public void onConnected(AdbShell.ConnectionMode mode) {
        if (mode == AdbShell.ConnectionMode.RSA) {
            // Connected with the saved RSA key.
        } else {
            // Connected after six-digit pairing.
        }
    }

    @Override
    public void onError(AdbShell.ConnectionError error, Throwable cause) {
        // Inspect error for the category and cause for the underlying exception.
    }
});
```

## Run a shell command

`run()` can be called from any class after initialization. It establishes a fresh
connection with the saved key, runs the command off the main thread, and returns
the result on the main thread.

```java
AdbShell.get().run("pm list packages", new AdbShell.Callback<String>() {
    @Override
    public void onSuccess(String output) {
        // Use the command output.
    }

    @Override
    public void onError(Throwable error) {
        // Handle the connection or command error.
    }
});
```

Pass only the device shell command, such as `id`, `getprop ro.product.model`, or
`settings get global adb_enabled`. Do not prefix it with `adb shell`.

Requires Android 11 (API 30) or newer.
