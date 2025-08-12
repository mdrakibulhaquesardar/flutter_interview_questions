### প্রশ্ন ১১: `BuildContext` কী?

**উত্তর (ডিটেইল):**

- Context হলো widget tree-তে Widget-এর লোকেশন/হ্যান্ডেল।
- `Navigator.of(context)`, `Theme.of(context)`, `MediaQuery.of(context)`—এসব context থেকে resolve হয়।
- Context scope সংবেদনশীল: child context থেকে parent-provided InheritedWidget অ্যাক্সেস সম্ভব, উল্টোটা নয়।

**Pitfall:** async টাস্কে context ধরে রেখে পরবর্তীতে ব্যবহার করলে widget dispose হলে ক্র্যাশ হতে পারে; `if (!mounted) return;` ব্যবহার করুন।

---

### প্রশ্ন ১২: MediaQuery এবং LayoutBuilder-এর পার্থক্য কী?

**উত্তর (ডিটেইল):**

- MediaQuery: ডিভাইস-উদ্ভুত মেট্রিক্স (screen size, padding, viewInsets, textScale)।
- LayoutBuilder: parent constraints (maxWidth/height) অনুযায়ী লেআউট অ্যাডাপ্ট।

**উদাহরণ:** কার্ডের গ্রিড কলাম সংখ্যা parent width অনুযায়ী পরিবর্তন করতে `LayoutBuilder` আদর্শ।

**Interview Tips:** orientation-ভিত্তিক বড় লেআউট বদল → MediaQuery; container-fit নির্ধারণ → LayoutBuilder।

---

### প্রশ্ন ১৩: `GlobalKey` কবে ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- একই উইজেটকে ট্রি-র বিভিন্ন জায়গায় মুভ করলেও state ধরে রাখতে বা দূর থেকে তার `State` অ্যাক্সেস করতে।
- উদাহরণ: `Form` validation (`GlobalKey<FormState>`), `ScaffoldMessenger`।

**Pitfall:** অতিরিক্ত GlobalKey পারফরম্যান্স ও কাপলিং খারাপ করে; লোকাল সমস্যায় লোকাল সমাধান (callback, controllers) ব্যবহার করুন।

---

### প্রশ্ন ১৪: `initState` vs `didChangeDependencies` কবে কোনটা?

**উত্তর (ডিটেইল):**

- `initState`: context-স্বাধীন one-time init (controller/ticker/logger)।
- `didChangeDependencies`: context-নির্ভর init (Theme/MediaQuery/Provider থেকে ভ্যালু পড়ে কাজ শুরু করা)।

**Note:** `didChangeDependencies` একাধিকবার কল হতে পারে—idempotent রাখুন।

---

### প্রশ্ন ১৫: `dispose()`-এ কী ক্লিনআপ করবেন?

**উত্তর (ডিটেইল):**

- `TextEditingController`, `ScrollController`, `AnimationController`, `FocusNode`, `StreamSubscription`, `Timer`—সব `dispose()`/`cancel()` করুন।
- নাহলে মেমরি/রিসোর্স লিক হবে, ব্যাকগ্রাউন্ড কাজ চলতেই থাকবে।

**Interview Tips:** `dispose`-এ async করবেন না; অর্ডার: নিজের রিসোর্স → `super.dispose()`।

