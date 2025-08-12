### প্রশ্ন ৩৬: `WillPopScope` কী কাজে লাগে?

**উত্তর (ডিটেইল):**

- সিস্টেম ব্যাক জেসচার/বাটন ইন্টারসেপ্ট করে `onWillPop`-এ `Future<bool>` রিটার্ন করে।
- true → pop হবে; false → থাকবে।

**Interview Tips:** unsaved changes কনফার্ম ডায়লগ, ডাবল-ব্যাক-টু-এক্সিট প্যাটার্ন।

---

### প্রশ্ন ৩৭: `OrientationBuilder` vs `MediaQuery.of(context).orientation`?

**উত্তর (ডিটেইল):**

- `OrientationBuilder`: orientation বদলালে নিজে রিবিল্ড করে; রেসপন্সিভ লেআউটে সুবিধা।
- `MediaQuery`: সরাসরি orientation পড়া; তবে নিজে রিবিল্ড ট্রিগার করে না।

**Interview Tips:** ডিপেন্ডেন্সি-ড্রিভেন UI হলে OrientationBuilder যথাযথ।

---

### প্রশ্ন ৩৮: `Hero` animation কীভাবে কাজ করে?

**উত্তর (ডিটেইল):**

- সোর্স/ডেস্টিনেশন `Hero(tag: ...)` মেলালে রাউট ট্রানজিশনে shared element animation হয়।
- `flightShuttleBuilder` দিয়ে কাস্টম ট্রানজিশন ভিজ্যুয়াল সম্ভব।

**Interview Tips:** ট্যাগ ইউনিক/কনসিস্টেন্ট রাখুন; ক্লিপিং ইস্যুতে `transitionOnUserGestures`/`createRectTween` টিউন করুন।

---

### প্রশ্ন ৩৯: `AnimatedContainer` কবে ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- কনফিগ প্রপার্টি (padding, margin, color, borderRadius) বদলালে স্মুথ ট্রানজিশন।
- `duration`, `curve` কাস্টমাইজযোগ্য।

**Interview Tips:** ছোট মাইক্রো-ইন্টার‌্যাকশনে subtle অ্যানিমেশন UX উন্নত করে।

---

### প্রশ্ন ৪০: FPS/জ্যাঙ্ক কিভাবে ট্রাবলশুট করবেন?

**উত্তর (ডিটেইল):**

- DevTools → Performance overlay, frame chart, CPU profiler; `flutter run --profile`।
- `debugProfilePaintsEnabled`, `repaintRainbowEnabled` দিয়ে repaint ট্র্যাক করুন (ডেভ-অনলি)।

**Interview Tips:** synchronous IO/মেইন-থ্রেড heavy কাজ/নেস্টেড লেআউট—প্রধান অপরাধী।


