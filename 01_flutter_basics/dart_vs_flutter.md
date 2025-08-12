### প্রশ্ন: Dart আর Flutter-এর পার্থক্য কী?

**উত্তর:**

- Dart: একটি প্রোগ্রামিং ল্যাঙ্গুয়েজ (Google)। AOT (release) ও JIT (dev)—দুইভাবেই কম্পাইল হয়, Null Safety সাপোর্টেড।
- Flutter: একটি UI framework/toolkit যা Dart দিয়ে লেখা। Widgets, Rendering, Gestures, Animation—সব মিলিয়ে অ্যাপ বানানোর ফ্রেমওয়ার্ক।
- সম্পর্ক: Flutter framework লিখতে/ব্যবহার করতে Dart লাগে; কিন্তু Dart আলাদা ভাবেও (CLI, server, web) ব্যবহার করা যায়।

**কী কী টেকনিক্যাল ডিফারেন্স মনে রাখবেন:**

- Language vs Framework → Dart (language), Flutter (framework + tooling)
- Compilation → Dart AOT/JIT; Flutter অ্যাপ Dart-কোড থেকে নেটিভ-কম্পাইলড বাইনারি তৈরি করে
- Ecosystem → Dart-এর প্যাকেজ ম্যানেজার pub.dev; Flutter-এর Widgets/Engine/Tooling

**উদাহরণ:** ছোট Dart কোড বনাম Flutter কোড

```dart
// Pure Dart
class Greeter {
  final String name;
  Greeter(this.name);
  String greet() => 'Hello, $name!';
}

void main() {
  print(Greeter('Dart').greet());
}
```

```dart
// Flutter (uses Dart)
import 'package:flutter/material.dart';

void main() => runApp(const MaterialApp(home: Home()));

class Home extends StatelessWidget {
  const Home({super.key});
  @override
  Widget build(BuildContext context) => const Scaffold(
        body: Center(child: Text('Hello from Flutter')),
      );
}
```

**Interview Tips / কখন কাজে লাগে:**

- যদি জিজ্ঞেস করে “Flutter কোন ভাষায় লিখি?” → Dart.
- “Dart কেন?” → Sound null safety, AOT/JIT, tooling, fast dev cycle.
- “Flutter কি শুধু মোবাইল?” → না; web/desktop/embedded পর্যন্ত।


