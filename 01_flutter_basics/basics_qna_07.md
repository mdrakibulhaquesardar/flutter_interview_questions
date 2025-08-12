### প্রশ্ন ৩১: `ListView.builder`-এ itemCount না দিলে কী হয়?

**উত্তর (ডিটেইল):**

- childBuilderDelegate যদি childCount ছাড়া থাকে, বিল্ডার আনবাউন্ডেড কল হতে পারে (পারফ ও মেমরি সমস্যা)।
- সর্বদা `itemCount` দিন, বা স্লিভার-ভিত্তিক ডেলিগেটে `childCount` দিন।

**Interview Tips:** বড় লিস্টে pagination/`ListView.separated`/`SliverList` বিবেচনা করুন।

---

### প্রশ্ন ৩২: `ListTile` কাস্টমাইজ কিভাবে করবেন?

**উত্তর (ডিটেইল):**

- কনফিগ: `leading`, `title`, `subtitle`, `trailing`, `isThreeLine`, `dense`, `selected`, `onTap`।
- থিমিং: `ListTileTheme`/`ThemeData.listTileTheme`।

**Interview Tips:** খুব কাস্টম UI হলে নিজে Row/Column বানান পারফরম্যান্স ও নিয়ন্ত্রণের জন্য।

---

### প্রশ্ন ৩৩: Image লোডিং অপ্টিমাইজেশন কীভাবে?

**উত্তর (ডিটেইল):**

- নেটওয়ার্ক ইমেজে ক্যাশিং: `cached_network_image`।
- ডিকোড কস্ট কমাতে `cacheWidth/height` বা `ResizeImage` ব্যবহার।
- `precacheImage` দিয়ে আগে থেকে লোড।

**Interview Tips:** বড় ইমেজে OOM এড়াতে রিসাইজ করুন; শিমার/প্লেসহোল্ডার UX উন্নত করে।

---

### প্রশ্ন ৩৪: `Future.delayed` কোথায় ইউজফুল?

**উত্তর (ডিটেইল):**

- ডিবাউন্স/থ্রোটলিং, টোস্ট/স্ন্যাকবার ডিলে, ট্রানজিশন-ফ্রেন্ডলি ওয়েট।
- টেস্টে async সিচুয়েশন সিমুলেট।

**Interview Tips:** প্রোডাকশনে অকারণে আর্টিফিশিয়াল ডিলে যুক্ত করবেন না।

---

### প্রশ্ন ৩৫: `showDialog`-এ context কোনটা দেবেন?

**উত্তর (ডিটেইল):**

- স্ক্রিন-লেভেলের context দিন যাতে সঠিক Navigator পাওয়া যায়। Nested navigator থাকলে `useRootNavigator` ব্যবহার করুন।
- Barrier dismiss, `Navigator.pop(context)` দিয়ে ক্লোজ।

**Interview Tips:** ডায়লগ-বিল্ডার আলাদা রাখুন; back press behavior/hardware back হ্যান্ডেল করুন।


