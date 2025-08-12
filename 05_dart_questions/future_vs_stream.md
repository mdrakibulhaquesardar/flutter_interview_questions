# Future vs Stream

এই টপিকের বিস্তারিত প্রশ্নোত্তর দেখুন:

- [Dart Q&A Set 1](dart_qna_01.md) - Dart Language Fundamentals
- [Dart Q&A Set 2](dart_qna_02.md) - প্রশ্ন ৮: Async Programming (Future, Stream) কীভাবে handle করবেন?
- [Dart Q&A Set 3](dart_qna_03.md) - Advanced Dart Concepts

## সংক্ষিপ্ত উত্তর:

**Future**: Single asynchronous operation handle করে। Future complete হওয়ার পর rebuild হয়।

**Stream**: Continuous data stream handle করে। Stream-এ নতুন data আসলে rebuild হয়।

## পার্থক্য:

- **Future**: Single async operation, one-time rebuild
- **Stream**: Multiple async operations, multiple rebuilds
- **Future**: API calls, file operations
- **Stream**: Real-time data, user input

## Interview Tips:

- Future single value return করে
- Stream multiple values yield করে
- async/await code readable করে
- Error handling async operations-এ গুরুত্বপূর্ণ


