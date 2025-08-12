### প্রশ্ন ১১: Flutter এ `InheritedWidget` এবং `Provider` এর মধ্যে পার্থক্য কী?

**উত্তর:**

`InheritedWidget` হলো Flutter এর একটি বেস ক্লাস যা উইজেট ট্রিতে ডেটা দক্ষতার সাথে প্রচার করার জন্য ব্যবহৃত হয়। এটি একটি ইমিউটেবল ডেটা ধারণ করে এবং যখন ডেটা পরিবর্তন হয়, তখন এটি নির্ভরশীল উইজেটগুলিকে পুনর্নির্মাণ করার জন্য notify করে।

`Provider` হলো একটি প্যাকেজ যা `InheritedWidget` এর উপর ভিত্তি করে তৈরি এবং স্টেট ম্যানেজমেন্টকে আরও সহজ এবং আধুনিক করে তোলে। এটি বিভিন্ন ধরনের প্রোভাইডার (যেমন `ChangeNotifierProvider`, `FutureProvider`, `StreamProvider` ইত্যাদি) সরবরাহ করে যা বিভিন্ন পরিস্থিতিতে ডেটা পরিচালনার জন্য উপযোগী।

মূল পার্থক্যগুলি হলো:

*   **সহজ ব্যবহার:** `Provider` `InheritedWidget` এর তুলনায় ব্যবহার করা অনেক সহজ, বিশেষ করে জটিল ডেটা স্ট্রাকচার বা লজিক হ্যান্ডেল করার ক্ষেত্রে।
*   **টাইপ সেফটি:** `Provider` টাইপ সেফটি প্রদান করে, যা ডেটা অ্যাক্সেস করার সময় ভুল হওয়ার সম্ভাবনা কমিয়ে দেয়।
*   **কর্মক্ষমতা:** `Provider` সূক্ষ্ম-স্তরের অপটিমাইজেশন সরবরাহ করে, যা শুধুমাত্র প্রয়োজনীয় উইজেটগুলিকে পুনর্নির্মাণ করতে সাহায্য করে।
*   **বিভিন্ন ধরনের ডেটা:** `Provider` শুধুমাত্র সাধারণ ডেটা নয়, `ChangeNotifier`, `Future`, `Stream` ইত্যাদির মতো বিভিন্ন ধরনের ডেটা হ্যান্ডেল করতে পারে।

সংক্ষেপে, `InheritedWidget` হলো একটি নিম্ন-স্তরের মেকানিজম, যেখানে `Provider` হলো একটি উচ্চ-স্তরের অ্যাবস্ট্রাকশন যা `InheritedWidget` ব্যবহার করে স্টেট ম্যানেজমেন্ট সহজ করে তোলে।

### প্রশ্ন ১২: Flutter এ `FutureBuilder` এবং `StreamBuilder` কখন ব্যবহার করবেন?

**উত্তর:**

উভয়ই Flutter এ অ্যাসিঙ্ক্রোনাস ডেটা হ্যান্ডেল করার জন্য ব্যবহৃত উইজেট, তবে তাদের ব্যবহারের ক্ষেত্র ভিন্ন।

*   **`FutureBuilder`:** যখন আপনার একটি অ্যাসিঙ্ক্রোনাস অপারেশন থেকে একবার ডেটা আসবে বলে আশা করেন (যেমন একটি নেটওয়ার্ক রিকোয়েস্ট বা ডাটাবেস কোয়েরি), তখন আপনি `FutureBuilder` ব্যবহার করবেন। এটি একটি `Future` অবজেক্ট নেয় এবং `Future` এর অবস্থার (loading, error, data) উপর ভিত্তি করে UI তৈরি করে।

    
```dart
FutureBuilder<String>(
      future: fetchData(), // returns a Future<String>
      builder: (BuildContext context, AsyncSnapshot<String> snapshot) {
        if (snapshot.connectionState == ConnectionState.waiting) {
          return CircularProgressIndicator();
        } else if (snapshot.hasError) {
          return Text('Error: ${snapshot.error}');
        } else {
          return Text('Data: ${snapshot.data}');
        }
      },
    )
```

*   **`StreamBuilder`:** যখন আপনার একটি অ্যাসিঙ্ক্রোনাস ডেটা স্ট্রিম থেকে সময়ের সাথে সাথে একাধিকবার ডেটা আসবে বলে আশা করেন (যেমন WebSocket সংযোগ বা Firebase রিয়েলটাইম ডেটা), তখন আপনি `StreamBuilder` ব্যবহার করবেন। এটি একটি `Stream` অবজেক্ট নেয় এবং `Stream` এ নতুন ডেটা আসার সাথে সাথে UI আপডেট করে।

    
```dart
StreamBuilder<int>(
      stream: streamData(), // returns a Stream<int>
      builder: (BuildContext context, AsyncSnapshot<int> snapshot) {
        if (snapshot.connectionState == ConnectionState.waiting) {
          return CircularProgressIndicator();
        } else if (snapshot.hasError) {
          return Text('Error: ${snapshot.error}');
        } else if (snapshot.hasData) {
          return Text('Data: ${snapshot.data}');
        } else {
          return Text('No data');
        }
      },
    )
```

সংক্ষেপে, একবারের ডেটার জন্য `FutureBuilder` এবং সময়ের সাথে সাথে একাধিকবার ডেটার জন্য `StreamBuilder` ব্যবহার করা হয়।

### প্রশ্ন ১৩: Flutter এ Custom Painter কী এবং এটি কখন ব্যবহার করবেন?

**উত্তর:**

Custom Painter হলো Flutter এর একটি শক্তিশালী বৈশিষ্ট্য যা আপনাকে উইজেটগুলিতে কাস্টম গ্রাফিক্স আঁকতে দেয়। এটি `CustomPaint` উইজেটের সাথে একত্রে ব্যবহৃত হয়। `CustomPainter` ক্লাস দুটি প্রধান মেথড প্রদান করে: `paint` এবং `shouldRepaint`।

*   **`paint(Canvas canvas, Size size)`:** এই মেথডটি গ্রাফিক্স আঁকার জন্য ব্যবহৃত হয়। `Canvas` অবজেক্ট বিভিন্ন আঁকার অপারেশন (যেমন লাইন, সার্কেল, টেক্সট, ইমেজ ইত্যাদি) সরবরাহ করে এবং `Size` অবজেক্ট আঁকার ক্ষেত্রের আকার ধারণ করে।
*   **`shouldRepaint(CustomPainter oldDelegate)`:** এই মেথডটি নিয়ন্ত্রণ করে যে উইজেটটি কখন পুনর্নির্মাণ করা উচিত। এটি সাধারণত `false` রিটার্ন করে যদি আঁকা ডেটা অপরিবর্তিত থাকে, যা পারফরম্যান্স অপটিমাইজেশনে সাহায্য করে।

Custom Painter কখন ব্যবহার করবেন:

*   জটিল বা অনিয়মিত আকারের UI উপাদান তৈরি করার জন্য যা স্ট্যান্ডার্ড উইজেট দিয়ে তৈরি করা কঠিন।
*   চার্ট, গ্রাফ বা ডেটা ভিজ্যুয়ালাইজেশন তৈরি করার জন্য।
*   ইন্টারেক্টিভ অঙ্কন বা স্বাক্ষর বৈশিষ্ট্য তৈরি করার জন্য।
*   নির্দিষ্ট প্রভাব বা অ্যানিমেশন তৈরি করার জন্য যা স্ট্যান্ডার্ড উইজেট দিয়ে সম্ভব নয়।

উদাহরণ: একটি কাস্টম আকৃতি আঁকা

```dart
class MyCustomPainter extends CustomPainter {
  @override
  void paint(Canvas canvas, Size size) {
    final paint = Paint()
      ..color = Colors.blue
      ..style = PaintingStyle.fill;

    final path = Path();
    path.moveTo(size.width * 0.2, size.height * 0.2);
    path.lineTo(size.width * 0.8, size.height * 0.2);
    path.lineTo(size.width * 0.5, size.height * 0.8);
    path.close();

    canvas.drawPath(path, paint);
  }

  @override
  bool shouldRepaint(covariant MyCustomPainter oldDelegate) {
    return false; // Only repaint if necessary
  }
}

// In your widget tree:
CustomPaint(
  painter: MyCustomPainter(),
  child: Container(), // Optional child widget
)
```

### প্রশ্ন ১৪: Flutter এ Render Object কী এবং এর ভূমিকা কী?

**উত্তর:**

Render Object হলো Flutter এর রেন্ডারিং পাইপলাইনের একটি মূল উপাদান। এটি লেআউট এবং পেইন্টিং লজিক ধারণ করে। প্রতিটি উইজেট একটি Render Object তৈরি করে (স্টেটলেস উইজেটের জন্য সরাসরি নয়, তবে স্টেটফুল উইজেটের স্টেট অবজেক্ট থেকে)। Render Object উইজেট ট্রির সমান্তরালভাবে একটি Render Tree তৈরি করে।

Render Object এর প্রধান ভূমিকাগুলি হলো:

*   **লেআউট:** এর শিশুদের আকার এবং অবস্থান নির্ধারণ করা।
*   **পেইন্টিং:** নিজের এবং তার শিশুদের UI আঁকা।
*   **হিট টেস্টিং:** ব্যবহারকারীর ইনপুট (যেমন ট্যাপ) কোন উইজেটের সাথে সম্পর্কিত তা নির্ধারণ করা।

Render Object গুলি সরাসরি স্ক্রিনে আঁকা হয় না। পরিবর্তে, তারা `Layer` অবজেক্ট তৈরি করে যা কম্পোজিট করা হয় এবং শেষ পর্যন্ত GPU তে পাঠানো হয় আঁকার জন্য। Render Object গুলি অত্যন্ত পারফরম্যান্ট কারণ তারা অপ্রয়োজনীয় রিকম্পোজিশন এড়িয়ে চলে এবং সরাসরি লেআউট এবং পেইন্টিং হ্যান্ডেল করে।

আপনি যখন `CustomPainter` ব্যবহার করেন, তখন আপনি আসলে একটি Render Object এর পেইন্টিং প্রক্রিয়াতে অংশ নিচ্ছেন।

### প্রশ্ন ১৫: Flutter এ Sliver কী এবং কেন এটি ব্যবহার করা হয়?

**উত্তর:**

Sliver হলো স্ক্রোলযোগ্য অঞ্চলের একটি অংশ যা `CustomScrollView` এর মধ্যে ব্যবহার করা হয়। স্ট্যান্ডার্ড স্ক্রোলযোগ্য উইজেট (যেমন `ListView`, `GridView`) পুরো বিষয়বস্তু তৈরি করে এবং তারপর স্ক্রোল করে, যা বড় ডেটাসেটের জন্য অদক্ষ হতে পারে। Sliver গুলি শুধুমাত্র দৃশ্যমান বা কাছাকাছি দৃশ্যমান বিষয়বস্তু তৈরি করে, যা মেমরি ব্যবহার এবং কর্মক্ষমতা উন্নত করে।

Sliver কেন ব্যবহার করা হয়:

*   **দক্ষতা:** দীর্ঘ তালিকা বা গ্রিডের জন্য কর্মক্ষমতা অপটিমাইজ করার জন্য।
*   **কাস্টম স্ক্রোলিং ইফেক্ট:** স্ক্রোল করার সময় হেডার সঙ্কুচিত করা, প্রসারিত করা বা অন্যান্য জটিল ইফেক্ট তৈরি করার জন্য।
*   **বিভিন্ন স্ক্রোলযোগ্য উইজেট একত্রিত করা:** একটি একক স্ক্রোলযোগ্য ভিউতে `ListView`, `GridView` এবং অন্যান্য কাস্টম স্ক্রোলযোগ্য উপাদান একত্রিত করার জন্য।

সাধারণ ব্যবহৃত Sliver গুলি:

*   `SliverAppBar`: স্ক্রোল করার সময় সঙ্কুচিত বা প্রসারিত হওয়া অ্যাপ বার।
*   `SliverList`: একটি তালিকার মতো Sliver।
*   `SliverGrid`: একটি গ্রিডের মতো Sliver।
*   `SliverFillRemaining`: অবশিষ্ট স্ক্রোলযোগ্য স্থান পূরণ করে এমন একটি Sliver।

উদাহরণ: একটি `CustomScrollView` এ `SliverAppBar` এবং `SliverList` ব্যবহার করা

```dart
CustomScrollView(
  slivers: <Widget>[
    SliverAppBar(
      expandedHeight: 200.0,
      flexibleSpace: FlexibleSpaceBar(
        title: Text('Sliver App Bar'),
      ),
    ),
    SliverList(
      delegate: SliverChildBuilderDelegate(
        (BuildContext context, int index) {
          return ListTile(
            title: Text('Item $index'),
          );
        },
        childCount: 50,
      ),
    ),
  ],
)
```

### প্রশ্ন ১৬: Flutter এ Platform Channels কী এবং এটি কখন ব্যবহার করবেন?

**উত্তর:**

Platform Channels হলো Flutter এবং নেটিভ (Android বা iOS) কোডের মধ্যে যোগাযোগ করার একটি মেকানিজম। এটি আপনাকে Flutter অ্যাপ থেকে নেটিভ প্ল্যাটফর্ম API কল করতে বা নেটিভ কোড থেকে Flutter এ ডেটা পাঠাতে দেয়।

Platform Channels তিনটি প্রধান উপাদান নিয়ে গঠিত:

*   **MethodChannel:** Flutter থেকে নেটিভ কোডে মেথড কল করার জন্য ব্যবহৃত হয় এবং একটি রেজাল্ট রিটার্ন করে।
*   **EventChannel:** নেটিভ কোড থেকে Flutter এ ডেটা স্ট্রিম পাঠানোর জন্য ব্যবহৃত হয়।
*   **BasicMessageChannel:** Flutter এবং নেটিভ কোডের মধ্যে এলোমেলো ডেটা আদান-প্রদান করার জন্য ব্যবহৃত হয়।

Platform Channels কখন ব্যবহার করবেন:

*   প্ল্যাটফর্ম-নির্দিষ্ট বৈশিষ্ট্য অ্যাক্সেস করার জন্য যা Flutter ফ্রেমওয়ার্কে উপলব্ধ নয় (যেমন ক্যামেরা, GPS, সেন্সর, নেটিভ UI উপাদান)।
*   নেটিভ লাইব্রেরি বা SDK ব্যবহার করার জন্য।
*   বিদ্যমান নেটিভ কোডবেসের সাথে সংহত করার জন্য।

উদাহরণ: MethodChannel ব্যবহার করে নেটিভ কোড কল করা (Flutter সাইড)

```dart
import 'package:flutter/services.dart';

static const platform = MethodChannel('com.example.myapp/battery');

Future<void> getBatteryLevel() async {
  String batteryLevel;
  try {
    final int result = await platform.invokeMethod('getBatteryLevel');
    batteryLevel = 'Battery level: $result%.';
  } on PlatformException catch (e) {
    batteryLevel = "Failed to get battery level: '${e.message}'.";
  }
  print(batteryLevel);
}
```

নেটিভ সাইডে (Android/Kotlin):

```kotlin
import androidx.annotation.NonNull
import io.flutter.embedding.android.FlutterActivity
import io.flutter.embedding.engine.FlutterEngine
import io.flutter.plugin.common.MethodChannel
import android.content.Context
import android.content.ContextWrapper
import android.content.Intent
import android.content.IntentFilter
import android.os.BatteryManager
import android.os.Build

class MainActivity: FlutterActivity() {
  private val CHANNEL = "com.example.myapp/battery"

  override fun configureFlutterEngine(@NonNull flutterEngine: FlutterEngine) {
    super.configureFlutterEngine(flutterEngine)
    MethodChannel(flutterEngine.dartExecutor.binaryMessenger, CHANNEL).setMethodCallHandler {
      call, result ->
      if (call.method == "getBatteryLevel") {
        val batteryLevel = getBatteryLevel()

        if (batteryLevel != -1) {
          result.success(batteryLevel)
        } else {
          result.error("UNAVAILABLE", "Battery level not available.", null)
        }
      } else {
        result.notImplemented()
      }
    }
  }

  private fun getBatteryLevel(): Int {
    val batteryLevel: Int
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
      val batteryManager = getSystemService(Context.BATTERY_SERVICE) as BatteryManager
      batteryLevel = batteryManager.getIntProperty(BatteryManager.BATTERY_PROPERTY_CAPACITY)
    } else {
      val intent = ContextWrapper(applicationContext).registerReceiver(null, IntentFilter(Intent.ACTION_BATTERY_CHANGED))
      batteryLevel = (intent!!.getIntExtra(BatteryManager.EXTRA_LEVEL, -1) * 100 / intent.getIntExtra(BatteryManager.EXTRA_SCALE, -1))
    }
    return batteryLevel
  }
}
```

### প্রশ্ন ১৭: Flutter এ FFI কী এবং এটি কীভাবে কাজ করে?

**উত্তর:**

FFI (Foreign Function Interface) হলো Dart এর একটি বৈশিষ্ট্য যা Dart কোড থেকে সরাসরি নেটিভ লাইব্রেরিতে (যেমন C, C++, Rust) কোড কল করার অনুমতি দেয়। এটি Platform Channels এর চেয়ে কম অ্যাবস্ট্রাকশন সরবরাহ করে, যার ফলে এটি C/C++ লাইব্রেরিগুলির সাথে সরাসরি ইন্টারঅ্যাক্ট করার জন্য আরও উপযোগী।

FFI কীভাবে কাজ করে:

1.  **লোডিং নেটিভ লাইব্রেরি:** Dart কোড ডায়নামিকভাবে নেটিভ লাইব্রেরি লোড করে।
2.  **ফাংশন লুকআপ:** লাইব্রেরিতে থাকা নির্দিষ্ট ফাংশনগুলির জন্য পয়েন্টার প্রাপ্ত করে।
3.  **ফাংশন কল:** প্রাপ্ত পয়েন্টার ব্যবহার করে নেটিভ ফাংশন কল করে।
4.  **ডেটা টাইপ ম্যাপিং:** Dart এবং C ডেটা টাইপের মধ্যে ম্যাপিং পরিচালনা করে।

FFI কখন ব্যবহার করবেন:

*   বিদ্যমান নেটিভ C/C++ লাইব্রেরি ব্যবহার করার জন্য যা Platform Channels দিয়ে অ্যাক্সেস করা কঠিন বা অদক্ষ।
*   উচ্চ-পারফরম্যান্স কম্পিউটেশন বা সিস্টেম-স্তরের কার্যকারিতা যা Dart এ উপলব্ধ নয়।
*   কম্পিউটেশনালি ইন্টেন্সিভ কাজগুলির জন্য যেখানে নেটিভ পারফরম্যান্স প্রয়োজন।

FFI ব্যবহার Platform Channels এর চেয়ে বেশি জটিল এবং মেমরি ম্যানেজমেন্ট এবং অন্যান্য নেটিভ-স্তরের বিবরণের সাথে ডিল করার প্রয়োজন হতে পারে।

### প্রশ্ন ১৮: Flutter অ্যাপের পারফরম্যান্স অপটিমাইজ করার জন্য কিছু টিপস কী কী?

**উত্তর:**

Flutter অ্যাপের পারফরম্যান্স অপটিমাইজ করার জন্য কিছু গুরুত্বপূর্ণ টিপস হলো:

*   **কম উইজেট তৈরি করুন:** অপ্রয়োজনীয় উইজেট তৈরি করা এড়িয়ে চলুন। `const` কন্সট্রাক্টর ব্যবহার করুন যেখানে সম্ভব।
*   **সঠিক স্টেট ম্যানেজমেন্ট ব্যবহার করুন:** শুধুমাত্র প্রয়োজনীয় উইজেটগুলিকে রিবিল্ড করতে স্টেট ম্যানেজমেন্ট সলিউশন (যেমন Provider, Riverpod, BLoC) ব্যবহার করুন। `setState` কল করার সময় `SetState` এর স্কোপ ছোট রাখুন।
*   **লিস্ট এবং গ্রিডের জন্য `ListView.builder` এবং `GridView.builder` ব্যবহার করুন:** এটি শুধুমাত্র দৃশ্যমান আইটেমগুলি তৈরি করে, যা মেমরি ব্যবহার কমায়।
*   **অপ্রয়োজনীয় বিল্ড এড়িয়ে চলুন:** `shouldRepaint` এবং `updateShouldNotify` এর মতো মেথডগুলি ব্যবহার করে কখন উইজেট রিবিল্ড করা উচিত তা নিয়ন্ত্রণ করুন।
*   **অ্যানিমেশন অপটিমাইজ করুন:** `RepaintBoundary` ব্যবহার করুন যেখানে অ্যানিমেশন চলছে। জটিল অ্যানিমেশনের জন্য `AnimatedBuilder` ব্যবহার করুন।
*   **ইমেজ ক্যাশিং ব্যবহার করুন:** ইমেজ লোডিং কর্মক্ষমতা উন্নত করতে `cached_network_image` এর মতো লাইব্রেরি ব্যবহার করুন।
*   **জাঙ্ক থেকে মুক্ত থাকুন:** UI তে 60fps বা 120fps বজায় রাখার চেষ্টা করুন। পারফরম্যান্স ডায়াগনস্টিক টুলস ব্যবহার করে জাঙ্ক শনাক্ত করুন।
*   **প্রোফাইল মোডে ডিবাগ করুন:** পারফরম্যান্স সমস্যাগুলি শনাক্ত করার জন্য প্রোফাইল মোডে অ্যাপ চালান।
*   **মেমরি ব্যবহার মনিটর করুন:** মেমরি লিক এড়াতে মেমরি ব্যবহার ট্র্যাক করুন।
*   **অপ্রয়োজনীয় অ্যাসেট এবং প্যাকেজগুলি সরান:** অ্যাপের আকার কমাতে অব্যবহৃত অ্যাসেট এবং প্যাকেজগুলি সরান।

### প্রশ্ন ১৯: Flutter এ মেমরি লিক কীভাবে শনাক্ত এবং সমাধান করবেন?

**উত্তর:**

মেমরি লিক ঘটে যখন অ্যাপ দ্বারা আর ব্যবহৃত না হওয়া মেমরি মুক্ত করা হয় না, যা সময়ের সাথে সাথে মেমরি ব্যবহার বৃদ্ধি করে এবং অ্যাপ ক্র্যাশ বা কর্মক্ষমতা সমস্যা সৃষ্টি করতে পারে।

Flutter এ মেমরি লিক শনাক্ত এবং সমাধান করার জন্য:

*   **Flutter Performance Tools ব্যবহার করুন:** Flutter DevTools এ মেমরি ট্যাব ব্যবহার করে অ্যাপের মেমরি ব্যবহার ট্র্যাক করুন। আপনি এখানে আবর্জনা সংগ্রহ (garbage collection) কার্যকলাপ এবং মেমরি বরাদ্দ দেখতে পারেন।
*   **হি L্যালোকেশনস মনিটর করুন:** DevTools এ হি L্যালোকেশনস ট্যাব আপনাকে দেখতে দেবে কোন ধরনের অবজেক্ট মেমরিতে বেশি স্থান নিচ্ছে।
*   **ডিসপোজযোগ্য অবজেক্টগুলি ডিসপোজ করুন:** `State` অবজেক্ট, `AnimationController`, `StreamSubscription`, `ChangeNotifier` ইত্যাদির মতো অবজেক্টগুলির জন্য `dispose()` মেথড কল করতে ভুলবেন না যখন তাদের আর প্রয়োজন হয় না।
*   **আনসাবস্ক্রাইব করুন:** `StreamSubscription` গুলি ঠিকমতো আনসাবস্ক্রাইব করতে ভুলবেন না যখন তাদের আর প্রয়োজন হয় না।
*   **সার্কুলার রেফারেন্স এড়িয়ে চলুন:** অবজেক্টগুলির মধ্যে সার্কুলার রেফারেন্স তৈরি করা এড়িয়ে চলুন যা গার্বেজ কালেক্টরকে মেমরি মুক্ত করা থেকে বাধা দিতে পারে।
*   **প্রোফাইলিং ব্যবহার করুন:** প্রোফাইল মোডে অ্যাপ চালিয়ে মেমরি লিক শনাক্ত করার চেষ্টা করুন।
*   **কোড রিভিউ করুন:** মেমরি ম্যানেজমেন্ট সম্পর্কিত সম্ভাব্য সমস্যাগুলির জন্য কোড রিভিউ করুন।

উদাহরণ: `ChangeNotifier` ডিসপোজ করা

```dart
class MyNotifier with ChangeNotifier {
  // ...
}

class MyWidget extends StatefulWidget {
  @override
  _MyWidgetState createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  final MyNotifier _notifier = MyNotifier();

  @override
  void dispose() {
    _notifier.dispose(); // Dispose the notifier
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // ...
  }
}
```

### প্রশ্ন ২০: Flutter এ Isolate এবং Future এর মধ্যে পার্থক্য কী?

**উত্তর:**

উভয়ই Flutter এ অ্যাসিঙ্ক্রোনাস প্রোগ্রামিং হ্যান্ডেল করার জন্য ব্যবহৃত হয়, তবে তাদের ব্যবহারের ক্ষেত্র এবং পদ্ধতি ভিন্ন।

*   **Future:** একটি `Future` একটি অ্যাসিঙ্ক্রোনাস অপারেশনের ফলাফলকে প্রতিনিধিত্ব করে যা ভবিষ্যতে উপলব্ধ হবে। এটি Dart এর ইভেন্ট লুপে চলে এবং প্রধান UI থ্রেডকে ব্লক করে না। `Future` ছোট বা মাঝারি আকারের অ্যাসিঙ্ক্রোনাস কাজের জন্য উপযুক্ত।

    
```dart
Future<String> fetchData() async {
      // Simulate a network request
      await Future.delayed(Duration(seconds: 2));
      return "Data from network";
    }

    void main() async {
      print("Fetching data...");
      String data = await fetchData();
      print(data);
    }
```

*   **Isolate:** একটি `Isolate` হলো একটি স্বাধীন কার্যনির্বাহী থ্রেড যার নিজস্ব মেমরি হিপ এবং ইভেন্ট লুপ রয়েছে। Isolate গুলি প্রধান UI থ্রেড থেকে সম্পূর্ণ আলাদাভাবে চলে এবং প্রধান থ্রেডকে ব্লক না করে দীর্ঘ-চলমান বা কম্পিউটেশনালি ইন্টেন্সিভ কাজ করার জন্য ব্যবহৃত হয়। Isolate গুলি মেসেজ পাসিংয়ের মাধ্যমে একে অপরের সাথে যোগাযোগ করে।

    
```dart
import 'dart:isolate';

    void complexComputation(SendPort sendPort) {
      // Perform a complex computation
      int result = 0;
      for (int i = 0; i < 1000000000; i++) {
        result += i;
      }
      sendPort.send(result);
    }

    void main() async {
      ReceivePort receivePort = ReceivePort();
      Isolate isolate = await Isolate.spawn(complexComputation, receivePort.sendPort);

      receivePort.listen((message) {
        print('Computation result: $message');
        receivePort.close();
        isolate.kill();
      });

      print("Isolate spawned...");
    }
    
```

সংক্ষেপে, `Future` হালকা অ্যাসিঙ্ক্রোনাস কাজের জন্য এবং `Isolate` ভারী, CPU-বাউন্ড কাজগুলির জন্য ব্যবহৃত হয় যা প্রধান UI থ্রেডকে ব্লক করতে পারে।