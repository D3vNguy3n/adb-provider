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

## API

```java
import com.dnturbo.adb.AdbShell;

AdbShell.get().connect(this, callback);
AdbShell.get().run("id", callback);
```

Requires Android 11 (API 30) or newer.
