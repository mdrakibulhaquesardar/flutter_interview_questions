### প্রশ্ন ২৬: `SafeArea` কেন ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- ডিভাইসের সিস্টেম ইনসেট (notch, status/navigation bars, gesture areas) এড়িয়ে কনটেন্ট সেফ জোনে রেন্ডার করে।
- বিকল্প: `MediaQuery.of(context).padding` ম্যানুয়ালি যোগ করে নিজে সেফ প্যাডিং হ্যান্ডেল করা।

**Interview Tips:** ফর্ম/রিডেবল কনটেন্টে সেফ জোন অপরিহার্য; ইমারসিভ মিডিয়ায় কেস-বাই-কেস ডিসিশন।

---

### প্রশ্ন ২৭: `Expanded` আর `Flexible` পার্থক্য কী?

**উত্তর (ডিটেইল):**

- দুটোই `Flex` (`Row`/`Column`) ভিত্তিক। `Expanded` child-কে বাকি স্পেস পূরণে বাধ্য করে; `Flexible` child-কে flex constraints-এর মধ্যে সাইজ হতে দেয়।
- `flex:` দিয়ে প্রপোরশন কন্ট্রোল করুন (যেমন 2:1)।

**Tip:** intrinsic সাইজ দরকার হলে `Flexible(fit: FlexFit.loose)` ব্যবহার করুন।

---

### প্রশ্ন ২৮: `SizedBox` vs `Container` কবে কোনটা?

**উত্তর (ডিটেইল):**

- কেবল সাইজ/স্পেসার → `SizedBox`।
- ডেকরেশন/প্যাডিং/অ্যালাইনমেন্ট → আলাদা আলাদা উইজেট নিন (`Padding`, `Align`, `DecoratedBox`) বা প্রয়োজন হলে `Container`।

**Interview Tips:** অপ্রয়োজনীয় generic `Container` এড়িয়ে স্পেশালাইজড উইজেট বেছে নিন (পারফরম্যান্স/রিডেবিলিটি)।

---

### প্রশ্ন ২৯: `GestureDetector` আর `InkWell` পার্থক্য?

**উত্তর (ডিটেইল):**

- `GestureDetector`: tap/pan/long-press ইত্যাদি জেসচার ডিটেক্ট করে; ভিজ্যুয়াল রিপল নেই।
- `InkWell`: Material ripple/touch feedback দেয়; parent-এ `Material` থাকা জরুরি।

**Interview Tips:** Pure Material UX → `InkWell`; কাস্টম ভিজ্যুয়াল হলে `GestureDetector` + নিজের ইফেক্ট।

---

### প্রশ্ন ৩০: `ClipRRect`/`ClipPath` কেন সাবধানে?

**উত্তর (ডিটেইল):**

- ক্লিপিং অতিরিক্ত হলে GPU ওভারহেড বাড়ে; লেয়ারের কম্পোজিশন জটিল হয়।

**Interview Tips:** শুধুমাত্র যেখানে দৃশ্যমানভাবে প্রয়োজন সেখানেই ক্লিপ করুন; DevTools দিয়ে overdraw/frames প্রোফাইল করুন।


