### প্রশ্ন ৪১: Performance pitfalls—state management-এ কোন ভুলগুলো সাধারণ?

**উত্তর (ডিটেইল):**

- গ্লোবাল বড় অবজেক্টে পুরো UI `watch`/`listen` করা।
- এক উইজেটে অনেক লিসেন/রিবিল্ড; স্প্লিট না করা।
- ইম্যুটেবিলিটি না রাখা → ইকুয়ালিটি ভাঙা।

**Interview Tips:** প্রোফাইলিং করে টার্গেট করুন; অকাল অপ্টিমাইজেশন নয়।

---

### প্রশ্ন ৪২: Memory leaks কিভাবে ধরবেন/এড়াবেন?

**উত্তর (ডিটেইল):**

- ডিসপোজেবল রিসোর্স (controller/stream) ডিসপোজ; লিসনার আনসাবস্ক্রাইব।
- DevTools memory tab, allocations timeline, leak tracking (tooling)।

**Interview Tips:** `autoDispose`/`mounted` চেক অভ্যাসে আনুন।

---

### প্রশ্ন ৪৩: Concurrency—ডুপ্লিকেট ফেচ/রেস কন্ডিশন কিভাবে হ্যান্ডেল করবেন?

**উত্তর (ডিটেইল):**

- রিকোয়েস্ট ডিডুপ (in-flight map), latest-only (switchMap/restartable), cancel tokens।
- UI থেকে রিকোয়েস্ট ট্রিগার থ্রোটল/ডিবাউন্স করুন।

**Interview Tips:** BLoC-এ `bloc_concurrency`; Riverpod-এ `ref.keepAlive` + guard।

---

### প্রশ্ন ৪৪: Modular state management—মাল্টিপল প্যাকেজ/ফিচার কম্পোজ কিভাবে?

**উত্তর (ডিটেইল):**

- প্রতিটি প্যাকেজ নিজস্ব প্রোভাইডার/ব্লক এক্সপোর্ট করে; অ্যাপ কম্পোজিশনে wire।
- নেমিং/সার্কুলার ডিপেন্ডেন্সি এড়াতে abstraction boundary রাখুন।

**Interview Tips:** ফিচার-লোকাল স্টেট গ্লোবাল স্কোপে লিক হতে দেবেন না।

---

### প্রশ্ন ৪৫: Migration strategy—Provider → Riverpod/BLoC কিভাবে ধাপে ধাপে?

**উত্তর (ডিটেইল):**

- নতুন ফিচার নতুন স্ট্যাকে; পুরোনো ধীরে ধীরে র‍্যাপার দিয়ে মাইগ্রেট।
- shared APIs/Repo লেয়ার শেয়ার রাখুন।

**Interview Tips:** ডুয়াল-রান পিরিয়ড রাখুন; টেস্ট কাভারেজ অনিবার্য।


