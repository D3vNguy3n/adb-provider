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
    implementation "io.github.d3vnguy3n:adb-provider:1.0.1"
}
```

## API

```java
import com.dnturbo.adb.AdbShell;

AdbShell.get().connect(this, callback);
AdbShell.get().run("id", callback);
```

Requires Android 11 (API 30) or newer.
