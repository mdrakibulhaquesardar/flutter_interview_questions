### প্রশ্ন ২৬: Selector/Select কিভাবে রিবিল্ড কমায়?

**উত্তর (ডিটেইল):**

- Provider: `Selector`/`context.select` দিয়ে অবজেক্টের নির্দিষ্ট অংশ লিসেন করলে শুধুমাত্র সেই ফিল্ড বদলালে রিবিল্ড হবে।
- ইকুয়ালিটি/কম্প্যারিজন সঠিক হলে অপ্রয়োজনীয় ফ্রেম বাঁচে।

**Interview Tips:** টাইল-ভিত্তিক লিস্টে প্রতিটি টাইলের স্ট্যাটাস আলাদা select করুন।

---

### প্রশ্ন ২৭: Riverpod-এ `select` এবং `ProviderListener` কবে ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

- `ref.watch(provider.select(...))`: নির্দিষ্ট সাব-ফিল্ডে রিঅ্যাক্ট।
- `ProviderListener`/`ref.listen`: সাইড-ইফেক্ট (নেভিগেশন/ডায়লগ)।

**Interview Tips:** UI বিল্ড ভ্যালু এবং সাইড-ইফেক্ট আলাদা রাখুন।

---

### প্রশ্ন ২৮: BLoC-এ `transformEvents`/`transformTransitions` কবে দরকার?

**উত্তর (ডিটেইল):**

- ইভেন্ট স্ট্রিমে ডিবাউন্স/থ্রোটল/মার্জিং; ট্রানজিশন লগিং/ফিল্টারিং।

**Interview Tips:** সার্চ/টাইপঅ্যাহেডে ডিবাউন্স অপরিহার্য।

---

### প্রশ্ন ২৯: State management এ ডিবাগিং টুলস?

**উত্তর (ডিটেইল):**

- Provider: `provider_logger`, Flutter DevTools rebuild tracker।
- Riverpod: `riverpod_graph`, `dart devtools` integration, provider observer।
- BLoC: `BlocObserver`/transition logging।

**Interview Tips:** সিগনেচার লগ/স্ন্যাপশট টেস্টিং যুক্ত করুন।

---

### প্রশ্ন ৩০: Error handling স্ট্র্যাটেজি?

**উত্তর (ডিটেইল):**

- ফিচার-স্টেট-এ error ফিল্ড/ট্যাগ যুক্ত করুন; UI-তে সুন্দর রেন্ডার।
- Global error boundary (FlutterError.onError), লগিং/ক্র্যাশ রিপোর্টিং সংযুক্ত।

**Interview Tips:** user-friendly মেসেজ, রিট্রাই/রিপোর্ট অপশন দিন।


