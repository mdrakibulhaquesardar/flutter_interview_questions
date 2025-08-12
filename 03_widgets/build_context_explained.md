# BuildContext Explained

এই টপিকের বিস্তারিত প্রশ্নোত্তর দেখুন:

- [Widgets Q&A Set 1](widgets_qna_01.md) - প্রশ্ন ২: BuildContext কী এবং এটা কেন গুরুত্বপূর্ণ?
- [Widgets Q&A Set 2](widgets_qna_02.md) - Layout এবং Styling Widgets
- [Widgets Q&A Set 3](widgets_qna_03.md) - Advanced Widget Concepts

## সংক্ষিপ্ত উত্তর:

**BuildContext** হলো widget tree-এর একটি handle যা widget-কে তার parent, ancestor, এবং global services-এর সাথে connect করে।

## মূল ভূমিকা:

- Theme, MediaQuery, Navigator access
- Provider/Riverpod state access
- Parent widget-এর সাথে communication
- Localization access

## Interview Tips:

- BuildContext = widget-এর identity card
- Always use the context from build method
- Context-কে class field হিসেবে store করবেন না


