## ডার্ট প্রশ্ন ও উত্তর (পর্ব ০৭)

**প্রশ্ন ১:** ডার্টে error handling এর জন্য `try-catch-finally` ব্লক কিভাবে ব্যবহার করা হয়?

**উত্তর:** ডার্টে রানটাইম ত্রুটি (runtime errors) বা exception handle করার জন্য `try-catch-finally` ব্লক ব্যবহার করা হয়।

*   `try` ব্লকের মধ্যে সম্ভাব্য error সৃষ্টিকারী কোড লেখা হয়।
*   যদি `try` ব্লকের মধ্যে কোনো exception ঘটে, তবে `catch` ব্লকের কোড execute হয়। `catch` ব্লকে আপনি exception object অ্যাক্সেস করতে পারেন এবং error অনুযায়ী logic implement করতে পারেন।
*   `finally` ব্লকের কোড সবসময় execute হয়, exception ঘটুক বা না ঘটুক। এটি cleanup operation এর জন্য useful।

উদাহরণ:

```
dart
void processData() {
  try {
    // কোড যা একটি error তৈরি করতে পারে
    int result = 10 ~/ 0; // ZeroDivisionError
    print('Result: $result');
  } catch (e) {
    // error handle করা
    print('An error occurred: $e');
  } finally {
    // সবসময় execute হবে
    print('Processing finished.');
  }
}

void main() {
  processData();
}
```
**প্রশ্ন ২:** ডার্টে custom exceptions কিভাবে তৈরি এবং ব্যবহার করা হয়?

**উত্তর:** ডার্টে custom exception তৈরি করতে, আপনি একটি নতুন ক্লাস তৈরি করতে পারেন যা `Exception` ক্লাসকে extend করে বা `Error` ক্লাসকে implement করে। তারপর আপনি আপনার custom logic অনুযায়ী exception throw করতে পারেন।

উদাহরণ:
```
dart
class InvalidInputException implements Exception {
  final String message;
  InvalidInputException(this.message);

  @override
  String toString() {
    return 'InvalidInputException: $message';
  }
}

void processInput(int value) {
  if (value < 0) {
    throw InvalidInputException("Input cannot be negative.");
  }
  print("Valid input: $value");
}

void main() {
  try {
    processInput(-5);
  } catch (e) {
    print(e);
  }
}
```
**প্রশ্ন ৩:** ডার্টে `rethrow` keyword এর ব্যবহার কি এবং কেন এটি দরকার?

**উত্তর:** `rethrow` keyword একটি `catch` ব্লকের মধ্যে ব্যবহার করা হয়। এটি বর্তমান exception টিকে পুনরায় throw করে দেয়, যাতে এটি higher-level code দ্বারা handle করা যায়। এটি তখন দরকার হয় যখন আপনি একটি exception কে catch করতে চান (যেমন লগ করার জন্য), কিন্তু আবার সেই exception টিকে propagation করতে চান যাতে অন্য অংশ এটি handle করতে পারে।

উদাহরণ:
```
dart
void readFile(String path) {
  try {
    // ফাইল রিডিং কোড
    throw FormatException("Invalid file format");
  } catch (e) {
    print("Error reading file: $e");
    rethrow; // exception টিকে পুনরায় throw করে
  }
}

void main() {
  try {
    readFile("data.txt");
  } catch (e) {
    print("Main catch block: Handled error: $e");
  }
}
```
**প্রশ্ন ৪:** ডার্টে `List` কী এবং এর কিছু সাধারণ operations বলুন।

**উত্তর:** `List` হলো ডার্টের একটি ordered collection of objects। এটি dynamic length হতে পারে এবং বিভিন্ন data type এর element ধারণ করতে পারে।

সাধারণ operations:

*   `add()`: List এ একটি element যোগ করে।
*   `remove()`: List থেকে নির্দিষ্ট element remove করে।
*   `removeAt()`: নির্দিষ্ট index এর element remove করে।
*   `length`: List এর length প্রদান করে।
*   `[]`: index ব্যবহার করে element অ্যাক্সেস করে।
*   `indexOf()`: নির্দিষ্ট element এর index প্রদান করে।
*   `contains()`: List এ নির্দিষ্ট element আছে কিনা তা check করে।
*   `sort()`: List এর element গুলো sort করে।

উদাহরণ:
```
dart
void main() {
  List<int> numbers = [1, 2, 3, 4, 5];
  numbers.add(6);
  print(numbers); // [1, 2, 3, 4, 5, 6]
  numbers.remove(3);
  print(numbers); // [1, 2, 4, 5, 6]
  print(numbers.length); // 5
  print(numbers[0]); // 1
}
```
**প্রশ্ন ৫:** ডার্টে `Map` কী এবং এর কিছু সাধারণ operations বলুন।

**উত্তর:** `Map` হলো ডার্টের একটি unordered collection of key-value pairs। প্রতিটি key unique হতে হবে।

সাধারণ operations:

*   `[]`: Key ব্যবহার করে value অ্যাক্সেস বা set করে।
*   `containsKey()`: Map এ নির্দিষ্ট key আছে কিনা তা check করে।
*   `containsValue()`: Map এ নির্দিষ্ট value আছে কিনা তা check করে।
*   `keys`: Map এর সমস্ত keys এর iterable প্রদান করে।
*   `values`: Map এর সমস্ত values এর iterable প্রদান করে।
*   `length`: Map এর key-value pairs এর সংখ্যা প্রদান করে।
*   `remove()`: নির্দিষ্ট key এর key-value pair remove করে।

উদাহরণ:

```
dart
void main() {
  Map<String, String> person = {
    'name': 'Alice',
    'city': 'New York',
  };
  print(person['name']); // Alice
  person['job'] = 'Engineer';
  print(person); // {name: Alice, city: New York, job: Engineer}
  print(person.containsKey('city')); // true
}
```
**প্রশ্ন ৬:** ডার্টে `Set` কী এবং এর কিছু সাধারণ operations বলুন।

**উত্তর:** `Set` হলো ডার্টের একটি unordered collection of unique elements। এটি duplicate element ধারণ করে না।

সাধারণ operations:

*   `add()`: Set এ একটি element যোগ করে। যদি element টি already থাকে, তবে এটি add হয় না।
*   `remove()`: Set থেকে নির্দিষ্ট element remove করে।
*   `contains()`: Set এ নির্দিষ্ট element আছে কিনা তা check করে।
*   `length`: Set এর element এর সংখ্যা প্রদান করে।
*   `union()`: দুটি Set এর union প্রদান করে।
*   `intersection()`: দুটি Set এর intersection প্রদান করে।
*   `difference()`: দুটি Set এর difference প্রদান করে।

উদাহরণ:
```
dart
void main() {
  Set<int> numbers = {1, 2, 3, 4, 5};
  numbers.add(3); // 3 already exists, so not added
  print(numbers); // {1, 2, 3, 4, 5}
  numbers.remove(2);
  print(numbers); // {1, 3, 4, 5}
  print(numbers.contains(4)); // true
}
```
**প্রশ্ন ৭:** ডার্টে Generics কী এবং কেন এটি ব্যবহার করা হয়?

**উত্তর:** Generics ডার্টে type safety প্রদান করে। এটি আপনাকে compile-time এ types enforce করতে দেয়, যা runtime errors কমাতে সাহায্য করে। Generics আপনাকে এমন ক্লাস, interface, বা function তৈরি করতে দেয় যা বিভিন্ন types এর সাথে কাজ করতে পারে।

উদাহরণ:
```
dart
// Generic List
List<int> intList = [1, 2, 3];
List<String> stringList = ["hello", "world"];

// Generic function
T firstElement<T>(List<T> list) {
  return list[0];
}

void main() {
  print(firstElement(intList)); // 1
  print(firstElement(stringList)); // hello
}
```
**প্রশ্ন ৮:** ডার্টে null safety কী?

**উত্তর:** Null safety ডার্টের একটি feature যা non-nullable types কে default করে। এর মানে হলো, যদি আপনি explicitly একটি variable কে nullable declare না করেন, তবে এটি `null` হতে পারবে না। এটি runtime null reference errors কমাতে সাহায্য করে।

Non-nullable type declaration এর জন্য আপনি variable type এর পরে `?` চিহ্ন ব্যবহার করেন না (যেমন `int`, `String`)। Nullable type declaration এর জন্য আপনি `?` চিহ্ন ব্যবহার করেন (যেমন `int?`, `String?`)।

**প্রশ্ন ৯:** Null safety এর সুবিধা কি কি?

**উত্তর:** Null safety এর প্রধান সুবিধাগুলো হলো:

*   **Reduced Runtime Errors:** এটি null reference errors হওয়ার সম্ভাবনা কমিয়ে দেয়, যা অ্যাপ Crash এর একটি সাধারণ কারণ।
*   **Improved Developer Productivity:** কম্পাইলার compile-time এ null related issues ধরিয়ে দেয়, যা debugging সময় বাঁচায়।
*   **Clearer Code Intent:** কোড দেখে বোঝা যায় কোন variable null হতে পারে আর কোনটি পারে না, যা কোডের রিডেবিলিটি বাড়ায়।
*   **Smaller Code Size:** Null checks compile-time এ করা হয়, তাই runtime এ অতিরিক্ত null checks এর প্রয়োজন হয় না, যা কোডের আকার ছোট করে।

**প্রশ্ন ১০:** ডার্টে non-nullable variable কে কিভাবে safely handle করা যায় যখন এটি null হতে পারে?

**উত্তর:** যদিও non-nullable variable default ভাবে null হয় না, কিন্তু কিছু ক্ষেত্রে (যেমন deserialization এর সময়) একটি value null আসতে পারে যা আপনি একটি non-nullable variable এ assign করতে চান। এই ক্ষেত্রে আপনি বিভিন্ন null-aware operators ব্যবহার করতে পারেন:

*   **`??` (Null-aware null coalescing operator):** যদি বাম পাশের expression null হয়, তবে ডান পাশের expression এর value ব্যবহার করে।
*   **`?.` (Null-aware access operator):** যদি বাম পাশের object null না হয়, তবে member access করে।
*   **`??=` (Null-aware assignment operator):** যদি variable null হয়, তবে value assign করে।
*   **`!` (Non-nullable assertion operator):** developer কম্পাইলারকে assert করে যে expression টি null হবে না। এটি সাবধানে ব্যবহার করা উচিত কারণ যদি এটি null হয় তবে runtime error হবে।

উদাহরণ:
