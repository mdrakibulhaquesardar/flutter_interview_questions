### প্রশ্ন ৩৬: Provider/Riverpod/BLoC—কোনটা শিখব/ব্যবহার করব?

**উত্তর (ডিটেইল):**

- ছোট/মিড অ্যাপ, দ্রুত ডেলিভারি → Provider/Riverpod।
- জটিল ইভেন্ট-ড্রিভেন/টিমে কঠোর কন্ট্র্যাক্ট → BLoC।
- Riverpod আর্কিটেকচারালি পরিষ্কার, কনটেক্সট-মুক্ত, টেস্টেবিলিটি চমৎকার।

**Interview Tips:** টিমের বিদ্যমান স্ট্যাক/মেইনটেনেন্স বিবেচনায় সিদ্ধান্ত।

---

### প্রশ্ন ৩৭: State serialization/hydration কিভাবে?

**উত্তর (ডিটেইল):**

- freezed/JSON serializable দিয়ে state মডেল সিরিয়ালাইজ; লোকাল স্টোরেজে সেভ করে স্টার্টে রিস্টোর।
- BLoC: `hydrated_bloc`। Riverpod: কাস্টম persistence layer।

**Interview Tips:** স্কিমা পরিবর্তনে মাইগ্রেশন/ভার্সনিং রাখুন।

---

### প্রশ্ন ৩৮: Error boundary/Global handling কিভাবে?

**উত্তর (ডিটেইল):**

- `runZonedGuarded`/`FlutterError.onError` এ ক্যাচ; স্টেট-লেভেলে এরর রেন্ডার।
- ক্র্যাশ রিপোর্টিং (Sentry/Crashlytics) ইন্টেগ্রেট।

**Interview Tips:** ইউজার-ফ্রেন্ডলি fallback UI দিন।

---

### প্রশ্ন ৩৯: State-driven navigation (deep link/guard) কিভাবে?

**উত্তর (ডিটেইল):**

- Router 2.0/go_router-এ auth state observe করে redirect; ব্লক/প্রোভাইডার লিসেন করে নেভিগেট।

**Interview Tips:** স্টেট পরিবর্তনে একাধিক নেভিগেশন ট্রিগার এড়াতে single source/guard।

---

### প্রশ্ন ৪০: Testing strategy—unit/widget/integration কোনটা কবে?

**উত্তর (ডিটেইল):**

- ইউনিট: নোটিফায়ার/ব্লক/রিপোজিটরি লজিক।
- উইজেট: UI + স্টেট ইন্টার‌অ্যাকশন।
- ইন্টিগ্রেশন: অ্যাপ ফ্লো/নেটওয়ার্ক/স্টোরেজ।

**Interview Tips:** ফাস্ট ফিডব্যাকের জন্য ইউনিট বেশি; ক্রিটিকাল ফ্লো ইন্টিগ্রেশন।


