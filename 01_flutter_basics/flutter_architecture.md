### প্রশ্ন: Flutter-এর আর্কিটেকচার/লেয়ারগুলো কীভাবে কাজ করে?

**উত্তর (হাই-লেভেল ৩ লেয়ার):**

1) Framework (Dart)
- Widgets → Element → RenderObject ট্রি
- Gestures, Animation, Rendering pipeline

2) Engine (C/C++ + Skia)
- Skia দিয়ে drawing/rasterization
- Dart runtime (AOT/JIT), text, accessibility, graphics primitives

3) Embedder (Platform-specific)
- iOS/Android/Web/Desktop-এর window/input/keyboard/back button ইত্যাদি
- Platform Channel এর মাধ্যমে নেটিভ API কল

**রেন্ডারিং পাইপলাইন সংক্ষেপে:**

- build → layout → paint → compositing → raster
- `setState`/state change হলে প্রাসঙ্গিক সাবট্রি রিবিল্ড হয়

**উদাহরণ:** Platform Channel (খুব সংক্ষিপ্ত আইডিয়া)

```dart
// Dart side
const channel = MethodChannel('device/info');
Future<String?> getDeviceName() => channel.invokeMethod('getName');
```

নেটিভ সাইডে একই চ্যানেলে `getName` হ্যান্ডেল করে ভ্যালু রিটার্ন করা হয়।

**Interview Tips / কখন কাজে লাগে:**

- “Why Flutter performance is good?” → নিজস্ব রেন্ডারিং (Skia), predictable frame pipeline.
- “Widget-Element-RenderObject পার্থক্য?” → Widget: config, Element: runtime instance, RenderObject: layout/paint logic.
- “নেটিভ ফিচার?” → Platform Channel/FFI.


