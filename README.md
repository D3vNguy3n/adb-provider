# ADB Provider

Android ADB pairing and shell library published as a Maven repository.

## Gradle

Add the repository to `settings.gradle`:

```gradle
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        maven {
            url = uri("https://raw.githubusercontent.com/D3vNguy3n/adb-provider/main")
        }
    }
}
```

Add the dependency to the app module:

```gradle
dependencies {
    implementation "com.dnturbo:adb-provider:1.0.0"
}
```

## API

```java
import com.dnturbo.adb.AdbShell;

AdbShell.get().connect(this, callback);
AdbShell.get().run("id", callback);
```

Requires Android 11 (API 30) or newer.
