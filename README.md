

---

# Flutter Native Integration Example

This project demonstrates how to integrate native Android code (Kotlin) with a Flutter app using method channels. The example functionality showcases JSON serialization and deserialization using the Gson library.

## Functionality Overview

The integrated functionality involves using Gson for JSON serialization/deserialization in the Flutter app. Gson is chosen for its robust and efficient JSON processing capabilities, which improve data handling and communication between Flutter and native code.

## Implementation Process

### Setting up Gson in Android Native Code

1. Added Gson as a dependency in the app-level build.gradle file.
2. Configured the MethodChannel in MainActivity.kt to handle Gson method calls.
3. Implemented methods to convert JSON to objects and vice versa using Gson.

### Communication with Flutter

1. **Method Channel Setup**:
   - Created a MethodChannel named "com.example.flutter_kotlin/gson" in Flutter.
   - Invoked methods on this channel to convert JSON to objects and vice versa.
   
2. **Handling Responses**:
   - Used `setState()` in Flutter to update the UI with the result received from the native code.

## Code Submission

### Android Native Code

#### MainActivity.kt:

```kotlin
import com.google.gson.Gson
import io.flutter.embedding.android.FlutterActivity
import io.flutter.embedding.engine.FlutterEngine
import io.flutter.plugin.common.MethodCall
import io.flutter.plugin.common.MethodChannel
import io.flutter.plugins.GeneratedPluginRegistrant

class MainActivity : FlutterActivity() {
    private val CHANNEL = "com.example.flutter_kotlin/gson"

    override fun configureFlutterEngine(flutterEngine: FlutterEngine) {
        super.configureFlutterEngine(flutterEngine)
        GeneratedPluginRegistrant.registerWith(flutterEngine)

        MethodChannel(flutterEngine.dartExecutor.binaryMessenger, CHANNEL).setMethodCallHandler { call, result ->
            when (call.method) {
                "toJson" -> {
                    try {
                        val obj = call.arguments as Map<String, Any>
                        val json = Gson().toJson(obj)
                        result.success(json)
                    } catch (e: Exception) {
                        result.error("SERIALIZATION_ERROR", e.message, null)
                    }
                }
                else -> result.notImplemented()
            }
        }
    }
}
```


