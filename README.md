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
    implementation "io.github.d3vnguy3n:adb-provider:1.0.6"
}
```

## API

```java
import com.dnturbo.adb.AdbShell;

AdbShell.get().connect(this, callback);
AdbShell.get().run("id", callback);
```

To customize the ADB/RSA device name, add this inside the app's
`<application>` element:

```xml
<meta-data
    android:name="com.dnturbo.adb.KEY_NAME"
    android:value="Any device name" />
```

Requires Android 11 (API 30) or newer.
