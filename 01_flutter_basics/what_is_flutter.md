### প্রশ্ন: Flutter কী?

**উত্তর:**

- Flutter হলো Google-এর একটি UI toolkit, যেটা দিয়ে একই কোডবেইস থেকে Mobile (iOS/Android), Web, Desktop—সব প্ল্যাটফর্মে নেটিভ-কম্পাইলড অ্যাপ বানানো যায়।
- প্রোগ্রামিং ল্যাঙ্গুয়েজ: Dart।
- পুরো UI হলো Widgets-এর সমন্বয়; নিজের রেন্ডারিং ইঞ্জিন (Skia) ব্যবহার করে।
- ডেভেলপার এক্সপেরিয়েন্স: Hot Reload/Hot Restart, দ্রুত ডেভেলপমেন্ট।

**উদাহরণ:** সবচেয়ে ছোট Flutter অ্যাপ

```dart
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});
  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: Scaffold(
        body: Center(child: Text('Hello Flutter')),
      ),
    );
  }
}
```

**Interview Tips / কখন কাজে লাগে:**

- “Why Flutter over React Native?” → নিজস্ব রেন্ডারিং ইঞ্জিন, কনসিস্টেন্ট UI, পারফরম্যান্স প্রেডিক্টেবল।
- “Native vs Flutter?” → Flutter নেটিভ-কম্পাইলড কোড জেনারেট করে; প্ল্যাটফর্ম চ্যানেল দিয়ে নেটিভ API কল করা যায়।
- “ব্যবহার কোথায়?” → MVP, স্টার্টআপ/এন্টারপ্রাইজ—যেখানে দ্রুত মাল্টি-প্ল্যাটফর্ম শিপমেন্ট দরকার।


