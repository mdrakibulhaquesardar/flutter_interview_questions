### প্রশ্ন ৬: Keys কী এবং কেন দরকার?

**উত্তর (ডিটেইল):**

- Keys ফ্রেমওয়ার্ককে বলে দেয় কোন Widget কোন Element/State-এর সাথে ম্যাচ করবে যখন sibling order/identity বদলায়।
- Without key: sibling reorder-এ ভুল state জুড়ে যেতে পারে।
- Key types: `ValueKey(value)` (স্টেবল ভ্যালু), `ObjectKey(obj)`, `UniqueKey()` (প্রতি বিল্ডে আলাদা)।

**উদাহরণ:** Reorderable লিস্টে প্রতিটি আইটেমে `ValueKey(item.id)` দিন।

**Interview Tips:** identity-sensitive UI (forms, animations) → Key আবশ্যক।

---

### প্রশ্ন ৭: InheritedWidget কী কাজ করে?

**উত্তর (ডিটেইল):**

- এটি হলো ট্রি-ডাউন ডাটা শেয়ার করার ভিত্তি। Dependents `context.dependOnInheritedWidgetOfExactType<T>()` কল করলে আপডেট সাবস্ক্রাইব হয়।
- নতুন ইনস্ট্যান্স দিলে dependents রিবিল্ড পায়।

**উদাহরণ:** Theme/MediaQuery/Localizations—সবই InheritedWidget।

**Interview Tips:** কাস্টম শেয়ার্ড স্টেট ইমপ্লিমেন্টে InheritedWidget/InheritedNotifier ব্যবহার করতে পারেন; সাধারণত Provider/Riverpod সহজ।

---

### প্রশ্ন ৮: `const` Widgets ব্যবহার কেন করবেন?

**উত্তর (ডিটেইল):**

- `const` কনস্ট্রাক্টর দিলে অবজেক্ট canonicalized হয়; একই ইনপুটে একই ইন্সট্যান্স reuse হয়, GC চাপ কমে।
- রিবিল্ডে `identical` সত্য হতে পারে, ফলে diff দ্রুত হয়।

**Pitfall:** যেকোনো একটি প্যারামেটার non-const হলে পুরো উইজেট const হবে না।

**Interview Tips:** স্ট্যাটিক সাবট্রি/আইকন/লেবেল/প্যাডিং—সবখানে `const` দিন।

---

### প্রশ্ন ৯: Navigator 1.0 vs 2.0 (Router API) পার্থক্য কী?

**উত্তর (ডিটেইল):**

- 1.0: `Navigator.push/pop`—স্ট্যাক ইম্পেরেটিভলি ম্যানেজ।
- 2.0 (Router): অ্যাপ স্টেট অনুযায়ী পেজ স্ট্যাক ডিক্লারেটিভ; প্ল্যাটফর্ম ব্যাক/URL সিঙ্ক সহজ।

**কখন কোনটা:**

- সিম্পল মোবাইল অ্যাপ → 1.0 যথেষ্ট।
- ওয়েব/ডীপ লিংক/কাস্টম ব্যাক স্ট্যাক → 2.0।

**Interview Tips:** `go_router`/`beamer` জনপ্রিয় 2.0 র‍্যাপার।

---

### প্রশ্ন ১০: Material vs Cupertino Widgets কখন ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- Material: গুগলের ডিজাইন সিস্টেম; বিস্তৃত উইজেট/থিম/অ্যানিমেশন সাপোর্ট।
- Cupertino: iOS হিউম্যান ইন্টারফেস গাইডলাইন ফলো করে।

**প্র্যাকটিক্যাল:** প্ল্যাটফর্ম চিহ্নিত করে কন্ডিশনাল UI দিন বা ক্রস-প্ল্যাটফর্ম অ্যাবস্ট্রাকশন প্যাকেজ ব্যবহার করুন।

**Interview Tips:** একই ফিচার দুইভাবে ইমপ্লিমেন্ট দেখাতে পারলে প্লাস পয়েন্ট।


