### প্রশ্ন ৩১: Undo/Redo বা optimistic update কিভাবে করবেন?

**উত্তর (ডিটেইল):**

- Optimistic: UI আগে আপডেট → API fail হলে রোলব্যাক। স্টেট মডেলে হিষ্ট্রি/পেন্ডিং অপস রাখুন।
- Undo stack: পূর্বের স্টেট স্ট্যাক/ডেক-এ রেখে এক ধাপে ফিরে যান।

**Interview Tips:** কনফ্লিক্ট/ডুপ রিকোয়েস্টে আইডেমপোটেন্সি টোকেন ব্যবহার।

---

### প্রশ্ন ৩২: Pagination/Infinite scroll state কিভাবে ম্যানেজ করবেন?

**উত্তর (ডিটেইল):**

- স্টেট: items, page, isLoading, hasMore, error।
- স্ক্রল থ্রেশহোল্ডে ফেচ; ডুপ্লিকেট কলে লক/ড্রপ।

**Interview Tips:** Riverpod autoDispose + keepAlive ক্যাশ; BLoC-এ droppable/restartable concurrency।

---

### প্রশ্ন ৩৩: Debounce/Throttle কোথায় বসাবেন?

**উত্তর (ডিটেইল):**

- Provider/Riverpod: notifier/controller লেয়ারে।
- BLoC: event ট্রান্সফরমারে বা রিপোজিটরিতে।

**Interview Tips:** সার্চ/টাইপঅ্যাহেডে debounce; বাটনে throttle।

---

### প্রশ্ন ৩৪: Cross-screen coordination (e.g., auth/login) কিভাবে?

**উত্তর (ডিটেইল):**

- Auth provider/bloc গ্লোবাল; স্ক্রিনরা watch/listen করে রিডাইরেক্ট নেয়।
- Deep-link/refresh scenario-তে single source of truth।

**Interview Tips:** guarding routes with provider/bloc-driven middleware।

---

### প্রশ্ন ৩৫: Feature modularization + DI স্ট্র্যাটেজি?

**উত্তর (ডিটেইল):**

- প্রতিটি ফিচার নিজের providers/blocs/routers/di module রাখে।
- Global composition root (ProviderScope/MultiProvider) থেকে wire করুন।

**Interview Tips:** cyclic dep এড়াতে interface + impl আলাদা করুন।


