### প্রশ্ন ২১: `StatelessWidget` কবে যথেষ্ট?

**উত্তর (ডিটেইল):**

- যখন UI কেবল প্রপস/উপরের স্টেটের ফাংশন; নিজে কোনো মিউটেবল স্টেট ম্যানেজ করে না।
- উদাহরণ: শিরোনাম/বাটন যা কলব্যাক নেয়, কিন্তু ভিতরে কাউন্টার রাখে না।

**Interview Tips:** অপ্রয়োজনীয় Stateful ব্যবহার এড়াতে লজিক উপরে তুলুন (lift state up)।

---

### প্রশ্ন ২২: `StatefulWidget`-এ expensive অপারেশন কোথায় রাখবেন?

**উত্তর (ডিটেইল):**

- একবারের ইনিট (DB ওপেন, কন্ট্রোলার/অ্যানিমেশন সেটআপ) → `initState`।
- context-ডিপেন্ডেন্ট ইনিট (Locale/Theme/Provider) → `didChangeDependencies`।
- `build`-এ CPU-heavy কাজ এড়ান; memoize করুন বা isolate/compute ব্যবহার করুন।

---

### প্রশ্ন ২৩: ListView vs Column+SingleChildScrollView?

**উত্তর (ডিটেইল):**

- বড়/ডায়নামিক ডাটা → `ListView.builder`/`SliverList` (লেজি)।
- অল্প, মিশ্র কনটেন্ট → `SingleChildScrollView` + `Column`।

**Tip:** nested scroll এ `CustomScrollView` + slivers বেশি নিয়ন্ত্রিত।

---

### প্রশ্ন ২৪: `FutureBuilder` কী কাজে লাগে?

**উত্তর (ডিটেইল):**

- এককালীন async অপারেশনের ফলাফল দেখাতে; connectionState অনুযায়ী লোডিং/এরর/ডাটা UI।

**Pitfall:** একই Future বারবার তৈরি হলে বারবার কল হবে; Future-কে `initState`-এ তৈরি করে পাস করুন।

---

### প্রশ্ন ২৫: `StreamBuilder` কবে ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- রিয়েল-টাইম আপডেট/ইভেন্ট ফিড (WebSocket, Firestore, BLE)।
- `initialData` দিন যাতে প্রথম ফ্রেমে UI স্ন্যাপ থাকে।

**Interview Tips:** বড় ডাটা স্ট্রিমে ব্যাকপ্রেশার/ডিবাউন্স কনসিডার করুন।


