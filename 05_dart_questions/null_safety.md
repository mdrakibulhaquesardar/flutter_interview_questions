# Null Safety

এই টপিকের বিস্তারিত প্রশ্নোত্তর দেখুন:

- [Dart Q&A Set 1](dart_qna_01.md) - প্রশ্ন ২: Null Safety কী এবং এটা কীভাবে implement করবেন?
- [Dart Q&A Set 2](dart_qna_02.md) - Collections এবং Generics
- [Dart Q&A Set 3](dart_qna_03.md) - Advanced Dart Concepts

## সংক্ষিপ্ত উত্তর:

**Null Safety** হলো Dart-এর একটি feature যা null reference errors prevent করে। এটা compile-time-এ null-related bugs catch করে।

## Null Safety Rules:

- Variables default-এ non-nullable
- Nullable variables-কে `?` দিয়ে mark করতে হয়
- Nullable variables-কে use করার আগে null check করতে হয়

## Key Features:

- **Non-nullable types**: Default behavior
- **Nullable types**: `?` suffix
- **Null-aware operators**: `??`, `?.`, `?..`
- **Late initialization**: `late` keyword

## Interview Tips:

- Null safety compile-time errors prevent করে
- `late` keyword runtime errors throw করে
- Null-aware operators code concise করে
- Required parameters non-nullable হওয়া উচিত


