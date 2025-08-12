# async/await

এই টপিকের বিস্তারিত প্রশ্নোত্তর দেখুন:

- [Dart Q&A Set 1](dart_qna_01.md) - Dart Language Fundamentals
- [Dart Q&A Set 2](dart_qna_02.md) - প্রশ্ন ৮: Async Programming (Future, Stream) কীভাবে handle করবেন?
- [Dart Q&A Set 3](dart_qna_03.md) - Advanced Dart Concepts

## সংক্ষিপ্ত উত্তর:

**async/await** হলো Dart-এ asynchronous programming-এর modern syntax। এটা asynchronous code-কে synchronous code-এর মত readable করে।

## Key Concepts:

- **async**: Function asynchronous হয়
- **await**: Wait for Future completion
- **Future**: Represents async operation
- **Stream**: Multiple async values

## Benefits:

- Code readability improve করে
- Error handling সহজ করে
- Synchronous code-এর মত structure
- Debugging সহজ করে

## Interview Tips:

- async function always Future return করে
- await শুধু async function-এ ব্যবহার করা যায়
- Error handling try-catch দিয়ে করা যায়
- Multiple await parallel execution support করে


