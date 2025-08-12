### প্রশ্ন ৪৬: অ্যাসেট (ইমেজ/ফন্ট) যোগ করার নিয়ম?

**উত্তর (ডিটেইল):**

- `pubspec.yaml`:

```yaml
flutter:
  assets:
    - assets/images/
  fonts:
    - family: Inter
      fonts:
        - asset: assets/fonts/Inter-Regular.ttf
```

- তারপর `flutter pub get`।

**Interview Tips:** পাথ কেস-সেন্সিটিভ; ওয়েবে ক্যাশ হেডার/নামিং স্ট্র্যাটেজি বিবেচনা।

---

### প্রশ্ন ৪৭: `pubspec.yaml`-এ `uses-material-design: true` মানে কী?

**উত্তর (ডিটেইল):**

- Material Icons ফন্ট/অ্যাসেট অন্তর্ভুক্ত করে এবং কিছু ডিফল্ট Material সেটিং সক্ষম করে।

**Interview Tips:** বাড়তি আইকন প্যাক দরকার হলে আলাদা ডিপেন্ডেন্সি অ্যাড করুন।

---

### প্রশ্ন ৪৮: Debug vs Profile vs Release মোড?

**উত্তর (ডিটেইল):**

- Debug: JIT, full asserts, hot reload, instrumentation—ডেভ-ফ্রেন্ডলি কিন্তু স্লো।
- Profile: AOT-সদৃশ পারফ, ট্রেসিং/প্রোফাইলিং অন; পারফ টেস্টিংয়ের জন্য।
- Release: AOT, মিনিমাম সাইজ/ম্যাক্স পারফ, asserts নেই, DevTools অফ।

**Interview Tips:** FPS/জ্যাঙ্ক মাপতে Profile/Release ইউজ করুন।

---

### প্রশ্ন ৪৯: Internationalization (i18n) বেসিক সেটআপ?

**উত্তর (ডিটেইল):**

- `flutter_localizations` অ্যাড; `flutter gen-l10n` বা `intl` দিয়ে ARB/লকেল ফাইল ম্যানেজ।
- `MaterialApp`-এ `localizationsDelegates`, `supportedLocales` সেট করুন।

**Interview Tips:** কোড-জেন (l10n) টাইপ-সেফ; প্লুরাল/জেন্ডার সাপোর্ট করে।

---

### প্রশ্ন ৫০: Accessibility (a11y) চেকলিস্ট কী?

**উত্তর (ডিটেইল):**

- যথাযথ semantics/labels, sufficient contrast, tap target ≥ 48dp, focus order সঠিক।
- ইমেজে `semanticLabel`/`excludeFromSemantics` সঠিকভাবে ব্যবহার।

**Interview Tips:** TalkBack/VoiceOver দিয়ে টেস্ট করুন; `Semantics` উইজেট প্রয়োগ করুন।


