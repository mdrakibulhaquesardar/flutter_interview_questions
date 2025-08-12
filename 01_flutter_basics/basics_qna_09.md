### প্রশ্ন ৪১: `CustomPainter` কী এবং কবে দরকার?

**উত্তর (ডিটেইল):**

- ক্যানভাসে ডাইরেক্ট ড্রইং (চার্ট, সিগনেচার প্যাড, ওয়েভ, গ্রাফ)।
- `paint(Canvas, Size)`-এ ড্র; `shouldRepaint` true হলে রেড্র।

**Interview Tips:** কষ্টসাধ্য ড্রইং সাবট্রি `RepaintBoundary`-তে রাখুন।

---

### প্রশ্ন ৪২: `RepaintBoundary` কেন গুরুত্বপূর্ণ?

**উত্তর (ডিটেইল):**

- সাবট্রির repaint ইফেক্ট আইসোলেট করে; অপর অংশ repaint থেকে বাঁচে।
- `RepaintBoundary` অতিরিক্ত দিলে layer জটিলতা বাড়ে—প্রোফাইল করে দিন।

**Interview Tips:** স্ক্রোলেবল কার্ড/ইমেজ-গ্রিড/কাস্টম পেইন্টার সেকশনে উপকারী।

---

### প্রশ্ন ৪৩: `TickerProviderStateMixin` কী?

**উত্তর (ডিটেইল):**

- অ্যানিমেশন ফ্রেম সিঙ্কের জন্য `vsync` সরবরাহ করে; অফস্ক্রিনে টিক বন্ধ থাকে।
- একটি কন্ট্রোলার → `SingleTickerProviderStateMixin`; একাধিক → `TickerProviderStateMixin`।

**Interview Tips:** `dispose()`-এ controller.dispose() ভুলবেন না।

---

### প্রশ্ন ৪৪: `FocusNode`/কীবোর্ড ম্যানেজমেন্ট কিভাবে?

**উত্তর (ডিটেইল):**

- `FocusNode` অ্যাটাচ করে ফোকাস শিফট/ডিসমিস; `TextInputAction` দিয়ে কীবোর্ড অ্যাকশন কন্ট্রোল।
- ফর্মে `FocusScope.of(context).nextFocus()`/`unfocus()` ইউজ করুন।

**Interview Tips:** FocusNode ডিসপোজ করুন; কীবোর্ড ওভারলে এড়াতে সেফ এরিয়া/ইনসেট হ্যান্ডেল করুন।

---

### প্রশ্ন ৪৫: `Platform.isAndroid`/`Platform.isIOS` কখন এড়াবেন?

**উত্তর (ডিটেইল):**

- UI লজিকে প্ল্যাটফর্ম ব্রাঞ্চিং জটিল করে; `Theme.of(context).platform` বা অ্যাবস্ট্রাকশন প্যাকেজ ভালো।
- তবে প্লাগইন/Platform Channel/FFI-তে প্ল্যাটফর্ম চেক অপরিহার্য।

**Interview Tips:** `defaultTargetPlatform` (foundation) ব্যবহার করতে পারেন।


