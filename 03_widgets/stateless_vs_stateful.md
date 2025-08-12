# Stateless vs Stateful Widgets

এই টপিকের বিস্তারিত প্রশ্নোত্তর দেখুন:

- [Widgets Q&A Set 1](widgets_qna_01.md) - প্রশ্ন ১: StatelessWidget আর StatefulWidget-এর মধ্যে মূল পার্থক্য কী?
- [Widgets Q&A Set 2](widgets_qna_02.md) - Layout এবং Styling Widgets
- [Widgets Q&A Set 3](widgets_qna_03.md) - Advanced Widget Concepts

## সংক্ষিপ্ত উত্তর:

**StatelessWidget**: Immutable widget যা একবার তৈরি হলে পরিবর্তন হয় না। এতে কোন internal state নেই।

**StatefulWidget**: Mutable state সহ widget যা user interaction বা data change অনুযায়ী নিজেকে rebuild করতে পারে।

## কখন কোনটা ব্যবহার করবেন:

- **StatelessWidget**: UI static থাকলে, performance critical হলে
- **StatefulWidget**: UI dynamic থাকলে, user interaction handle করতে হলে


