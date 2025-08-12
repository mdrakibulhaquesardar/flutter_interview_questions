### প্রশ্ন ১: Hot Reload আর Hot Restart-এর পার্থক্য কী?

**উত্তর (ডিটেইল):**

- Hot Reload: Dart VM-এ কোড ইনজেক্ট হয় এবং চলমান widget tree পুনঃবিল্ড হয়। বেশিরভাগ লোকাল state (যেমন `State` অবজেক্ট) টিকে থাকে, তাই UI দ্রুত আপডেট দেখা যায়।
- Hot Restart: পুরো অ্যাপ নতুন করে স্টার্ট হয়, সব state রিসেট হয়, `main()` থেকে শুরু। ইনিট-লজিক/ডিপেনডেন্সি রেসেট দরকার হলে এটা ব্যবহার করুন।

**উদাহরণ:** থিম বা ছোট UI টুইক → Hot Reload; কনফিগ/DI/সিঙ্গলটন বদলালে → Hot Restart।

**Common pitfalls:** ক্লাস সিগনেচার বা জেনেরিক টাইপ বদলালে reload সবসময় কাজ নাও করতে পারে; তখন restart নিন।

**Interview Tips:** দ্রুত iteration → Reload; deterministic clean state → Restart।

---

### প্রশ্ন ২: Widget কী?

**উত্তর (ডিটেইল):**

- Widget হলো UI-এর declarative কনফিগারেশন। Widget নিজে হালকা/immutable; রানটাইমে Element এই Widget-এর লাইফসাইকেল ম্যানেজ করে এবং RenderObject লেআউট/পেইন্ট করে।
- Flutter-এ সবকিছু Widget: লেআউট (`Row`, `Column`), স্টাইলিং (`Padding`, `Container`), কনটেন্ট (`Text`, `Image`), ইন্টার‌্যাকশন (`GestureDetector`)।

**কেন গুরুত্বপূর্ণ:** Widget-কে ছোট, কম্পোজেবল, এবং single-responsibility রাখা হলে রিবিল্ড সস্তা হয় এবং কোড পড়তে সহজ হয়।

**Interview Tips:** Widget = config, Element = instance, RenderObject = geometry/paint — এ তিনটির ভূমিকায় স্পষ্ট থাকুন।

---

### প্রশ্ন ৩: StatefulWidget-এর lifecycle কীভাবে কাজ করে?

**উত্তর (ডিটেইল):**

- অর্ডার: `createState()` → `initState()` → `didChangeDependencies()` → `build()` → (props/state বদল) `didUpdateWidget()` → `setState()` → `build()` → ... → `deactivate()` → `dispose()`।
- `initState()`: one-time init (controller/ticker তৈরি)।
- `didChangeDependencies()`: InheritedWidget/Localizations বদলালে কল হয়।
- `didUpdateWidget()`: parent নতুন widget দিলে পুরনোটির সাথে ডিফ করা যায়।
- `dispose()`: controller/stream/timer/FocusNode ক্লিনআপ।

**উদাহরণ:**

```dart
late final ScrollController _c;

@override
void initState() {
  super.initState();
  _c = ScrollController();
}

@override
void dispose() {
  _c.dispose();
  super.dispose();
}
```

**Interview Tips:** context-নির্ভর init (Theme, MediaQuery, Provider) → `didChangeDependencies`; একাধিকবার কল হতে পারে—guard করুন।

---

### প্রশ্ন ৪: `build()` মেথডের ভূমিকা কী?

**উত্তর (ডিটেইল):**

- বর্তমান state/props অনুযায়ী একটি নতুন widget tree রিটার্ন করে। এটি pure হওয়া উচিত: IO, heavy compute, বা side-effect `build()`-এ করবেন না।
- একই ইনপুটে একই আউটপুট রাখলে রিবিল্ড প্রেডিক্টেবল হয়।

**উদাহরণ (ভুল বনাম ঠিক):**

```dart
// ভুল: build-এ heavy কাজ
Widget build(BuildContext context) {
  final data = fetchSync(); // block করে
  return Text('$data');
}

// ঠিক: init/futurebuilder ব্যবহার
late final Future<String> _data;
void initState() { _data = fetchAsync(); }
Widget build(BuildContext context) => FutureBuilder(
  future: _data,
  builder: (_, s) => Text('${s.data}')
);
```

**Interview Tips:** `build()` ছোট রাখুন; সাব-উইজেট/মেথডে ভাগ করুন।

---

### প্রশ্ন ৫: `setState` কীভাবে কাজ করে?

**উত্তর (ডিটেইল):**

- `setState(() { ... })`-এর মধ্যে state মিউটেশন করুন; Flutter ফ্রেমওয়ার্ক সেই সাবট্রি dirty মার্ক করে এবং পরের ফ্রেমে রিবিল্ড করে।
- এক ফ্রেমে বারবার `setState` করলে ব্যাচ হতে পারে; সম্ভব হলে আপডেট একসাথে করুন।

**কোড:**

```dart
onPressed: () {
  setState(() {
    counter += 1;
  });
}
```

**Common pitfalls:**

- async কাজ শেষ হওয়ার পর `setState` কলের আগে `mounted` চেক করুন।
- `setState`-এ async/await দেবেন না; await বাইরে করে রেজাল্ট নিয়ে ভিতরে আপডেট করুন।

**Interview Tips:** Ephemeral UI state → `setState`; shared/app state → state management (Provider/Riverpod/Bloc)।


