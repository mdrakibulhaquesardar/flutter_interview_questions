### প্রশ্ন ৪৬: Code organization—ফোল্ডার স্ট্রাকচার কেমন হওয়া উচিত?

**উত্তর (ডিটেইল):**

- by feature: `features/feature_x/{data,domain,presentation}`; প্রতিটিতে providers/blocs/views।
- shared: `core/{networking,storage,di,theme}`।

**Interview Tips:** ফিচার ফোল্ডার আইসোলেশন স্কেলিং সহজ করে।

---

### প্রশ্ন ৪৭: State versioning—breaking changes কিভাবে হ্যান্ডেল করবেন?

**উত্তর (ডিটেইল):**

- persisted state-এ `schemaVersion` রাখুন; মাইগ্রেশন স্টেপ লিখুন।
- ইনকম্প্যাটিবল হলে fallback/clear strategy।

**Interview Tips:** মাইগ্রেশন টেস্ট অপরিহার্য।

---

### 질문 ৪৮: Offline-first স্ট্র্যাটেজি state management-এ কিভাবে?

**উত্তর (ডিটেইল):**

- cache-then-network; conflict resolution; queue sync।
- স্টেট-এ `syncStatus`/`lastUpdated` রাখুন।

**Interview Tips:** ব্যাটারি/ডাটা কনস্ট্রেইন্টে ব্যাকঅফ/রিট্রাই।

---

### প্রশ্ন ৪৯: Security-sensitive স্টেট (টোকেন) কিভাবে রাখবেন?

**উত্তর (ডিটেইল):**

- in-memory short-lived; persisted হলে `flutter_secure_storage`/OS keystore।
- লগিং-এ রেডাক্ট; ইনজেকশনে সর্বনিম্ন স্কোপ।

**Interview Tips:** রিফ্রেশ-টোকেন ফ্লো ব্লকে/নোটিফায়ারে কেন্দ্রীভূত রাখুন।

---

### প্রশ্ন ৫০: Common anti-patterns কী কী?

**উত্তর (ডিটেইল):**

- God provider/bloc, UI-লজিক মিক্সিং, গ্লোবাল mutable স্টেট, uncontrolled listeners।
- async state লুকানোর জন্য silent drops; error state না দেখানো।

**Interview Tips:** ছোট কম্পোজেবল ইউনিট, ইম্যুটেবল স্টেট, স্পষ্ট ট্রানজিশন, টেস্ট-ফার্স্ট।


