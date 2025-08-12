**প্রশ্ন:** Dart-এ `factory` কনস্ট্রাক্টর কেন ব্যবহার করা হয়? এটি সাধারণ কনস্ট্রাক্টর থেকে কীভাবে আলাদা?

**উত্তর:** `factory` কনস্ট্রাক্টর একটি নতুন ইনস্ট্যান্স তৈরি করার পরিবর্তে বিদ্যমান ইনস্ট্যান্স রিটার্ন করতে পারে বা সাবক্লাসের ইনস্ট্যান্স রিটার্ন করতে পারে। এটি সিঙ্গেলটন প্যাটার্ন বাস্তবায়নের জন্য বা ফ্যাক্টরি মেথড ডিজাইন প্যাটার্ন ব্যবহার করার জন্য দরকারী।

সাধারণ কনস্ট্রাক্টর সর্বদা একটি নতুন ইনস্ট্যান্স তৈরি করে, যেখানে `factory` কনস্ট্রাক্টর রিটার্ন টাইপ নির্দিষ্ট করে না এবং একই ক্লাস বা এর সাবক্লাসের ইনস্ট্যান্স রিটার্ন করতে পারে।

```
dart
class MyClass {
  static final MyClass _instance = MyClass._internal();

  factory MyClass() {
    return _instance;
  }

  MyClass._internal();
}
```
---

**প্রশ্ন:** Dart-এর `async` এবং `await` কী এবং কীভাবে তারা asynchronous অপারেশন পরিচালনা করে?

**উত্তর:** `async` একটি ফাংশনকে মার্ক করে যা asynchronous অপারেশন সম্পাদন করবে। এটি বোঝায় যে ফাংশনটি একটি `Future` রিটার্ন করবে। `await` keyword একটি `async` ফাংশনের মধ্যে ব্যবহার করা হয় একটি `Future`-এর ফলাফল পাওয়ার জন্য। `await` কোড execution বন্ধ করে না, বরং এটি ইভেন্ট লুপকে অন্যান্য কাজ করার অনুমতি দেয় যতক্ষণ না `Future` সম্পূর্ণ হয়।

```
dart
Future<void> fetchData() async {
  print('Fetching data...');
  await Future.delayed(Duration(seconds: 2));
  print('Data fetched!');
}
```
---

**প্রশ্ন:** Dart-এ `Stream` কী? এটি `Future` থেকে কীভাবে আলাদা?

**উত্তর:** একটি `Stream` asynchronous ডেটার একটি সিকোয়েন্স প্রতিনিধিত্ব করে। একটি `Future` একটি সিঙ্গেল asynchronous ফলাফল প্রতিনিধিত্ব করে, যেখানে একটি `Stream` সময়ের সাথে সাথে একাধিক ফলাফল ডেলিভার করতে পারে। আপনি `Stream`-এর ডেটা শোনার জন্য `.listen()` মেথড ব্যবহার করতে পারেন।

---

**প্রশ্ন:** Dart-এ `yield` কী এবং `async*` ফাংশনে এটি কীভাবে ব্যবহৃত হয়?

**উত্তর:** `yield` keyword `async*` জেনারেটর ফাংশনগুলিতে ব্যবহৃত হয়। এটি একটি স্ট্রিমে একটি নতুন ডেটা পয়েন্ট পুশ করে। `async*` ফাংশনগুলি Stream রিটার্ন করে এবং `yield` ব্যবহার করে ডেটা ইমিট করে।

```
dart
Stream<int> countStream(int max) async* {
  for (int i = 1; i <= max; i++) {
    yield i;
  }
}
```
---

**প্রশ্ন:** Dart-এ Extension Methods কী? একটি উদাহরণ দিন।

**উত্তর:** Extension methods আপনাকে বিদ্যমান লাইব্রেরিতে নতুন কার্যকারিতা যুক্ত করার অনুমতি দেয়, এমনকি যদি আপনার কাছে সেই লাইব্রেরির সোর্স কোড না থাকে। এটি কোড পঠনযোগ্যতা এবং reuseability উন্নত করতে সাহায্য করে।
```
dart
extension StringExtensions on String {
  String capitalize() {
    if (isEmpty) {
      return this;
    }
    return "${this[0].toUpperCase()}${substring(1)}";
  }
}

void main() {
  String name = "flutter";
  print(name.capitalize()); // Output: Flutter
}
```
---

**প্রশ্ন:** Dart-এ Mixin কী? কীভাবে এটি ব্যবহার করা হয়?

**উত্তর:** Mixin হলো একাধিক ক্লাস heirarchy জুড়ে কোড reuse করার একটি উপায়। একটি Mixin নিজে ইনস্ট্যান্সিয়েট করা যায় না, তবে এটি `with` keyword ব্যবহার করে ক্লাসের সাথে যুক্ত করা যেতে পারে। Mixin ক্লাসের মধ্যে মেথড এবং ভ্যারিয়েবল থাকতে পারে যা যুক্ত করা ক্লাসে ব্যবহার করা যেতে পারে।
```
dart
mixin CanFly {
  void fly() {
    print('Can fly!');
  }
}

class Bird with CanFly {
  // Bird class can now use the fly() method
}
```
---

**প্রশ্ন:** Dart-এ `typedef` কী? এটি কেন দরকারী?

**উত্তর:** `typedef` একটি ফাংশন টাইপের জন্য একটি alias তৈরি করার অনুমতি দেয়। এটি কোডকে আরও পঠনযোগ্য করে তোলে, বিশেষ করে যখন ফাংশন টাইপগুলি জটিল হয়। এটি callbacks বা ইভেন্ট হ্যান্ডলারের জন্য প্রায়ই ব্যবহৃত হয়।
```
dart
typedef IntOperation = int Function(int x, int y);

int add(int a, int b) => a + b;
int subtract(int a, int b) => a - b;

void main() {
  IntOperation operation = add;
  print(operation(5, 3)); // Output: 8

  operation = subtract;
  print(operation(5, 3)); // Output: 2
}
```
---

**প্রশ্ন:** Dart-এ Testing এর গুরুত্ব কী? Unit Test, Widget Test এবং Integration Test এর মধ্যে পার্থক্য কী?

**উত্তর:** Testing সফটওয়্যার ডেভেলপমেন্ট প্রক্রিয়ার একটি অপরিহার্য অংশ যা নিশ্চিত করে যে আপনার কোড প্রত্যাশিতভাবে কাজ করছে এবং বাগগুলি কমিয়ে আনে।

*   **Unit Test:** একটি ছোট, স্বতন্ত্র কোড ইউনিট (যেমন একটি ফাংশন বা ক্লাস) পরীক্ষা করে। এটি সবচেয়ে দ্রুত এবং সহজ ধরনের টেস্ট।
*   **Widget Test:** একটি সিঙ্গেল উইজেট পরীক্ষা করে নিশ্চিত করে যে এটি UI heirarchy তৈরি করছে এবং প্রত্যাশিতভাবে interact করছে। এটি UI লজিক পরীক্ষা করার জন্য দরকারী।
*   **Integration Test:** আপনার অ্যাপের বিভিন্ন অংশ বা পুরো অ্যাপটি একসাথে পরীক্ষা করে নিশ্চিত করে যে তারা সঠিকভাবে ইন্টিগ্রেট হয়েছে এবং কাজ করছে। এটি ব্যবহারকারীর প্রবাহ অনুকরণ করে।

---

**প্রশ্ন:** Dart-এ Null Safety কী এবং এটি কীভাবে কাজ করে?

**উত্তর:** Null Safety একটি ভাষা বৈশিষ্ট্য যা আপনাকে null reference ত্রুটিগুলি এড়াতে সাহায্য করে। Dart-এর Null Safety ডিফল্টভাবে non-nullable, যার মানে একটি ভ্যারিয়েবল null হতে পারে না যদি না আপনি explicitly এটিকে nullable (`?`) হিসাবে ঘোষণা করেন। এটি কম্পাইল-টাইমেই অনেক null-সম্পর্কিত ত্রুটি ধরতে সাহায্য করে।
```
dart
String nonNullableString = "hello"; // Cannot be null
String? nullableString = null; // Can be null
```
---

**প্রশ্ন:** Dart-এ `covariant` keyword কী এবং কখন এটি ব্যবহার করা হয়?

**উত্তর:** `covariant` keyword একটি মেথড প্যারামিটারের টাইপ স্পেসিফাই করার জন্য ব্যবহৃত হয় যা সাবক্লাসগুলিতে একটি ভিন্ন, কিন্তু covariant টাইপ গ্রহণ করতে পারে। এটি Polymorphism এবং type safety বজায় রাখতে সাহায্য করে যখন আপনি একটি সুপারক্লাস মেথডকে সাবক্লাসে override করেন এবং প্যারামিটারের টাইপকে আরও স্পেসিফিক করতে চান।