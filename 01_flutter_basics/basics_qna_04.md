### প্রশ্ন ১৬: `const` constructor কিভাবে কাজ করে?

**উত্তর (ডিটেইল):**

- সব ফিল্ড `final`/immutable হলে এবং super-ও const হলে const constructor ডিফাইন করা যায়।
- Compile-time এ অবজেক্ট তৈরি; identical instances reuse হয় (canonicalization)।

**উদাহরণ:**

```dart
class Label {
  final String text;
  const Label(this.text);
}

const a = Label('Hi');
const b = Label('Hi');
assert(identical(a, b));
```

**Interview Tips:** nested `const` দিলে propagation সর্বোচ্চ হবে।

---

### প্রশ্ন ১৭: `final` vs `const` পার্থক্য?

**উত্তর (ডিটেইল):**

- `final`: একবার অ্যাসাইন হলে আর পরিবর্তন নয়; কিন্তু ভ্যালু runtime-এ রিসলভ হয়।
- `const`: compile-time এ ফ্রোজেন; লিটারাল/const কনস্ট্রাক্টর চাই।

**Interview Tips:** config values যা কখনো পাল্টাবে না → `const`; API result holder → `final`।

---

### প্রশ্ন ১৮: `main()` এ `runApp` কেন দরকার?

**উত্তর (ডিটেইল):**

- এটি Flutter framework-কে জানায় কোন root widget থেকে UI ট্রি শুরু হবে; binding/engine সেটআপ সম্পন্ন হয়।
- Async init প্রয়োজনে `WidgetsFlutterBinding.ensureInitialized()` আগে কল করুন।

**উদাহরণ:** Firebase init তারপর runApp।

---

### প্রশ্ন ১৯: `Scaffold` কী করে?

**উত্তর (ডিটেইল):**

- Material স্ক্রিন স্ট্রাকচার, যেখানে app bar, body, FAB, drawer, bottom sheet, snack bar ইন্টেগ্রেটেড।
- `ScaffoldMessenger` এর মাধ্যমে স্কাফোল্ড-অ্যাগনস্টিক snack bar শো করা যায়।

---

### প্রশ্ন ২০: `ThemeData` দিয়ে গ্লোবাল থিম কিভাবে সেট করবেন?

**উত্তর (ডিটেইল):**

- `MaterialApp(theme: ThemeData(...), darkTheme: ThemeData.dark(), themeMode: ThemeMode.system)` দিয়ে গ্লোবাল থিম।
- `Theme.of(context)`/`ColorScheme` ইউটিলাইজ করে কনসিস্টেন্ট কালার/টোন।

**Tip:** কাস্টম typography/shape/inputs স্ট্যান্ডার্ডাইজ করুন।


