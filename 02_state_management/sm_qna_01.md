### প্রশ্ন ১: `setState` vs state management লাইব্রেরি—কখন কোনটা?

**উত্তর (ডিটেইল):**

Flutter-এ স্টেট ম্যানেজমেন্টের জন্য `setState` এবং বিভিন্ন লাইব্রেরি (যেমন Provider, Riverpod, BLoC) উভয়ই ব্যবহার করা হয়। এদের ব্যবহারের ক্ষেত্র ভিন্ন:

**`setState`**:
- **কখন ব্যবহার করবেন**: যখন স্টেট শুধুমাত্র একটি নির্দিষ্ট `StatefulWidget` এবং তার সরাসরি চাইল্ড উইজেটগুলোর মধ্যে সীমাবদ্ধ থাকে (লোকাল বা এফিমেরাল UI স্টেট)। যেমন, একটি চেকবক্সের টগল স্টেট, একটি টেক্সট ফিল্ডের ইনপুট ভ্যালু, একটি ট্যাবের বর্তমান ইনডেক্স, বা একটি অ্যানিমেশনের স্টেট।
- **সুবিধা**: শেখা সহজ, দ্রুত ইমপ্লিমেন্ট করা যায়, এবং ছোট স্কোপের জন্য পারফরম্যান্ট।
- **সীমাবদ্ধতা**: অ্যাপের স্টেট বড় বা জটিল হলে, বা একাধিক স্ক্রিন/উইজেটের মধ্যে স্টেট শেয়ার করার প্রয়োজন হলে `setState` ব্যবহার করা কঠিন হয়ে পড়ে। এতে কোড ডুপ্লিকেশন, স্প্যাগেটি কোড এবং বাগ হওয়ার সম্ভাবনা বাড়ে।

**State Management লাইব্রেরি (Provider/Riverpod/BLoC)**:
- **কখন ব্যবহার করবেন**: যখন স্টেট একাধিক উইজেট বা স্ক্রিনের মধ্যে শেয়ার করার প্রয়োজন হয় (অ্যাপ-ওয়াইড বা শেয়ার্ড স্টেট), যখন অ্যাসিঙ্ক্রোনাস অপারেশন (যেমন API কল) থেকে আসা ডেটা ম্যানেজ করতে হয়, বা যখন বিজনেস লজিককে UI থেকে আলাদা করে টেস্টেবিলিটি এবং স্কেলেবিলিটি নিশ্চিত করতে হয়।
- **সুবিধা**: কোড অর্গানাইজেশন উন্নত করে, স্টেট পরিবর্তনকে প্রেডিক্টেবল করে, টেস্টিং সহজ করে, এবং বড় অ্যাপ্লিকেশনের মেইনটেনেন্স সহজ করে।
- **উদাহরণ**: ইউজার অথেন্টিকেশন স্টেট, শপিং কার্টের আইটেম, ডেটাবেস থেকে আসা ডেটা, থিম সেটিংস ইত্যাদি।

**Pitfalls:** `setState` দিয়ে অ্যাপ-ওয়াইড স্টেট ম্যানেজ করার চেষ্টা করলে "prop drilling" (প্রপসকে অনেক লেয়ার নিচে পাস করা), coupling (উইজেটগুলোর মধ্যে অতিরিক্ত নির্ভরশীলতা), ডুপ্লিকেশন এবং বাগ বাড়ে। এটি কোডকে অগোছালো এবং মেইনটেইন করা কঠিন করে তোলে।

**Interview Tips:** একটি সহজ হিউরিস্টিক হলো: যদি স্টেটটি শুধুমাত্র একটি উইজেটের জীবনচক্রের সাথে সম্পর্কিত হয় এবং অন্য কোনো উইজেটকে প্রভাবিত না করে, তাহলে `setState` ব্যবহার করুন। যদি স্টেটটি একাধিক উইজেট বা স্ক্রিনের মধ্যে শেয়ার করার প্রয়োজন হয়, অ্যাসিঙ্ক্রোনাস ডেটা জড়িত থাকে, বা বিজনেস লজিককে UI থেকে আলাদা করতে চান, তাহলে একটি স্টেট ম্যানেজমেন্ট লাইব্রেরি ব্যবহার করা শ্রেয়।

---

### প্রশ্ন ২: Provider কী? কোন সমস্যা সমাধান করে?

**উত্তর (ডিটেইল):**

**Provider** হলো Flutter-এর জন্য একটি জনপ্রিয় স্টেট ম্যানেজমেন্ট প্যাকেজ যা Google-এর ডেভেলপারদের দ্বারা প্রস্তাবিত। এটি মূলত Flutter-এর বিল্ট-ইন `InheritedWidget`-এর উপর ভিত্তি করে তৈরি, কিন্তু এটিকে ব্যবহার করা অনেক সহজ এবং আরও ফ্লেক্সিবল করে তোলে।

**Provider যে সমস্যাগুলো সমাধান করে:**

1. **Prop Drilling (প্রপ ড্রিলিং)**: Flutter-এ ডেটা সাধারণত উইজেট ট্রি-এর উপর থেকে নিচে পাস করা হয়। যদি একটি ডেটা অনেক নিচের একটি চাইল্ড উইজেটে দরকার হয়, তাহলে সেই ডেটাটিকে মাঝের সমস্ত উইজেট দিয়ে পাস করতে হয়, যদিও মাঝের উইজেটগুলোর সেই ডেটার প্রয়োজন নাও হতে পারে। এটি কোডকে অগোছালো এবং মেইনটেইন করা কঠিন করে তোলে। Provider এই সমস্যা সমাধান করে, কারণ এটি উইজেট ট্রি-এর যেকোনো জায়গা থেকে ডেটা অ্যাক্সেস করার সুযোগ দেয়, মাঝের উইজেটগুলোকে ডেটা পাস করার প্রয়োজন হয় না।

2. **Rebuild Optimization (রিবিল্ড অপ্টিমাইজেশন)**: `setState` ব্যবহার করলে প্রায়শই অপ্রয়োজনীয় উইজেট রিবিল্ড হয়, যা পারফরম্যান্সের উপর নেতিবাচক প্রভাব ফেলতে পারে। Provider স্মার্টলি শুধুমাত্র সেই উইজেটগুলোকে রিবিল্ড করে যারা স্টেটের পরিবর্তনের উপর নির্ভরশীল, যার ফলে পারফরম্যান্স উন্নত হয়।

3. **Dependency Injection (ডিপেন্ডেন্সি ইনজেকশন)**: Provider একটি সহজ এবং কার্যকর উপায় প্রদান করে অ্যাপ্লিকেশনের বিভিন্ন অংশে ডিপেন্ডেন্সি ইনজেক্ট করার জন্য। এর মাধ্যমে আপনি আপনার বিজনেস লজিক, সার্ভিসেস, বা ডেটা মডেলগুলোকে UI থেকে আলাদা রাখতে পারেন, যা কোডকে মডুলার এবং টেস্টেবল করে তোলে।

4. **Resource Management (রিসোর্স ম্যানেজমেন্ট)**: Provider স্বয়ংক্রিয়ভাবে রিসোর্সগুলো (যেমন `ChangeNotifier` বা `StreamSubscription`) ডিসপোজ করতে সাহায্য করে যখন সেগুলোর আর প্রয়োজন হয় না, যা মেমরি লিক প্রতিরোধে সহায়তা করে।

**Provider-এর বিভিন্ন প্রকার:**
Provider প্যাকেজে বিভিন্ন ধরনের প্রোভাইডার রয়েছে যা বিভিন্ন ব্যবহারের ক্ষেত্রে উপযোগী:
* `Provider<T>`: একটি রিড-অনলি ভ্যালু প্রদান করে।
* `ChangeNotifierProvider`: `ChangeNotifier` অবজেক্ট প্রদান করে এবং যখন `notifyListeners()` কল করা হয় তখন নির্ভরশীল উইজেটগুলোকে রিবিল্ড করে।
* `FutureProvider`: একটি `Future` থেকে আসা ডেটা হ্যান্ডেল করে (loading, error, data স্টেট)।
* `StreamProvider`: একটি `Stream` থেকে আসা ডেটা হ্যান্ডেল করে (loading, error, data স্টেট)।
* `ValueListenableProvider`: `ValueNotifier` থেকে আসা ডেটা হ্যান্ডেল করে।
* `ProxyProvider`/`ChangeNotifierProxyProvider`: একাধিক প্রোভাইডারের ভ্যালু থেকে একটি নতুন প্রোভাইডার তৈরি করতে ব্যবহৃত হয়।

**উদাহরণ:**
একটি সাধারণ কাউন্টার অ্যাপে `ChangeNotifierProvider` ব্যবহার:

```dart
// 1. স্টেট মডেল (ChangeNotifier)
class Counter extends ChangeNotifier {
  int value = 0;

  void increment() {
    value++;
    notifyListeners(); // লিসনারদের জানায় যে স্টেট পরিবর্তিত হয়েছে
  }
}

// 2. রুটে Provider সেটআপ
void main() {
  runApp(
    ChangeNotifierProvider( // Counter অবজেক্ট প্রদান করে
      create: (context) => Counter(),
      child: const MyApp(),
    ),
  );
}

// 3. UI-তে স্টেট ব্যবহার
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: const Text('Provider Counter')),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              const Text('You have pushed the button this many times:'),
              Text(
                // context.watch<Counter>() স্টেট পরিবর্তন হলে রিবিল্ড ট্রিগার করে
                '${context.watch<Counter>().value}',
                style: Theme.of(context).textTheme.headlineMedium,
              ),
            ],
          ),
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: () {
            // context.read<Counter>() শুধুমাত্র মেথড কল করার জন্য, রিবিল্ড করে না
            context.read<Counter>().increment();
          },
          child: const Icon(Icons.add),
        ),
      ),
    );
  }
}
```

**Interview Tips:** Provider ছোট থেকে মিড-স্কেল অ্যাপ্লিকেশনের জন্য একটি চমৎকার পছন্দ কারণ এটি শেখা সহজ, বয়লারপ্লেট কোড কম এবং যথেষ্ট ফ্লেক্সিবল। এটি `InheritedWidget`-এর জটিলতা লুকিয়ে একটি পরিষ্কার API প্রদান করে।

---

### প্রশ্ন ৩: `context.watch`, `context.read`, `context.select`—পার্থক্য কী?

**উত্তর (ডিটেইল):**

Provider-এ স্টেট অ্যাক্সেস করার জন্য তিনটি মূল মেথড রয়েছে, প্রতিটির আলাদা উদ্দেশ্য এবং ব্যবহারের ক্ষেত্র আছে:

**`context.watch<T>()`**:
- **কাজ**: একটি প্রোভাইডারকে "সাবস্ক্রাইব" করে এবং যখন সেই প্রোভাইডারের স্টেট পরিবর্তিত হয় তখন উইজেটকে রিবিল্ড করে।
- **কখন ব্যবহার করবেন**: যখন আপনি UI-তে স্টেটের পরিবর্তন দেখাতে চান। এটি সবচেয়ে সাধারণ ব্যবহার।
- **সতর্কতা**: `build` মেথডের বাইরে (যেমন `initState`, `didChangeDependencies`) ব্যবহার করবেন না, কারণ এটি ইনফিনিট লুপ তৈরি করতে পারে।

**`context.read<T>()`**:
- **কাজ**: একটি প্রোভাইডার থেকে ভ্যালু বা মেথড কল করে, কিন্তু স্টেট পরিবর্তন হলে রিবিল্ড করে না।
- **কখন ব্যবহার করবেন**: ইভেন্ট হ্যান্ডলার, কলব্যাক, বা `build` মেথডের বাইরে যখন শুধুমাত্র মেথড কল করতে হয়।
- **সুবিধা**: পারফরম্যান্স ভালো কারণ এটি রিবিল্ড ট্রিগার করে না।

**`context.select<T, R>(selector)`**:
- **কাজ**: একটি প্রোভাইডারের নির্দিষ্ট অংশ শুধুমাত্র পর্যবেক্ষণ করে এবং শুধুমাত্র সেই অংশ পরিবর্তিত হলে রিবিল্ড করে।
- **কখন ব্যবহার করবেন**: যখন একটি বড় অবজেক্টের শুধুমাত্র একটি ছোট অংশ পরিবর্তন হলে রিবিল্ড করতে চান।
- **সুবিধা**: অপ্রয়োজনীয় রিবিল্ড এড়িয়ে পারফরম্যান্স উন্নত করে।

**উদাহরণ:**

```dart
class User extends ChangeNotifier {
  String name = 'John';
  int age = 25;
  String email = 'john@example.com';

  void updateName(String newName) {
    name = newName;
    notifyListeners();
  }

  void updateAge(int newAge) {
    age = newAge;
    notifyListeners();
  }
}

class UserProfile extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // শুধুমাত্র নাম পরিবর্তন হলে রিবিল্ড হবে
    final userName = context.select<User, String>((user) => user.name);
    
    // বয়স পরিবর্তন হলে রিবিল্ড হবে না
    final userAge = context.select<User, int>((user) => user.age);
    
    return Column(
      children: [
        Text('Name: $userName'), // শুধুমাত্র নাম পরিবর্তন হলে আপডেট হবে
        Text('Age: $userAge'),   // বয়স পরিবর্তন হলে আপডেট হবে না
        ElevatedButton(
          onPressed: () {
            // রিবিল্ড করে না, শুধুমাত্র মেথড কল করে
            context.read<User>().updateName('Jane');
          },
          child: Text('Update Name'),
        ),
      ],
    );
  }
}
```

**Pitfalls:**
1. `build` মেথডের বাইরে `watch` ব্যবহার করলে ইনফিনিট লুপ হতে পারে।
2. `select`-এ কমপ্লেক্স লজিক রাখলে পারফরম্যান্স খারাপ হতে পারে।
3. `read` ব্যবহার করে স্টেট পরিবর্তন করলে UI আপডেট হবে না।

**Interview Tips:** 
- `watch` → UI আপডেটের জন্য
- `read` → ইভেন্ট হ্যান্ডলার/কলব্যাকের জন্য  
- `select` → পারফরম্যান্স অপ্টিমাইজেশনের জন্য

---

### প্রশ্ন ৪: `ChangeNotifier` vs `ValueNotifier`—কখন কোনটা?

**উত্তর (ডিটেইল):**

Flutter-এ স্টেট ম্যানেজমেন্টের জন্য `ChangeNotifier` এবং `ValueNotifier` দুটি গুরুত্বপূর্ণ ক্লাস, প্রতিটির আলাদা ব্যবহারের ক্ষেত্র আছে:

**`ChangeNotifier`**:
- **কাজ**: একাধিক ফিল্ড বা কমপ্লেক্স স্টেট ম্যানেজ করার জন্য ডিজাইন করা।
- **বৈশিষ্ট্য**: 
  - একাধিক প্রপার্টি থাকতে পারে
  - `notifyListeners()` কল করে সব লিসনারকে জানায়
  - গ্রানুলার কন্ট্রোল নেই (কোন ফিল্ড পরিবর্তিত হয়েছে তা জানা যায় না)
- **কখন ব্যবহার করবেন**: যখন একটি ক্লাসে একাধিক প্রপার্টি থাকে এবং সেগুলো পরিবর্তিত হতে পারে।

**`ValueNotifier<T>`**:
- **কাজ**: একটি সিঙ্গেল ভ্যালু কেন্দ্রিক স্টেট ম্যানেজ করার জন্য।
- **বৈশিষ্ট্য**:
  - শুধুমাত্র একটি `value` প্রপার্টি
  - `value` পরিবর্তিত হলে স্বয়ংক্রিয়ভাবে `notifyListeners()` কল হয়
  - হালকা এবং পারফরম্যান্ট
  - টাইপ সেফ
- **কখন ব্যবহার করবেন**: যখন শুধুমাত্র একটি ভ্যালু ট্র্যাক করতে হয়।

**উদাহরণ:**

```dart
// ChangeNotifier - একাধিক ফিল্ড
class UserProfile extends ChangeNotifier {
  String _name = '';
  int _age = 0;
  String _email = '';

  String get name => _name;
  int get age => _age;
  String get email => _email;

  void updateName(String newName) {
    _name = newName;
    notifyListeners(); // সব লিসনার জানবে
  }

  void updateAge(int newAge) {
    _age = newAge;
    notifyListeners(); // সব লিসনার জানবে
  }

  void updateEmail(String newEmail) {
    _email = newEmail;
    notifyListeners(); // সব লিসনার জানবে
  }
}

// ValueNotifier - একটি ভ্যালু
class Counter extends ValueNotifier<int> {
  Counter() : super(0);

  void increment() {
    value++; // স্বয়ংক্রিয়ভাবে notifyListeners() কল হয়
  }

  void decrement() {
    value--; // স্বয়ংক্রিয়ভাবে notifyListeners() কল হয়
  }
}

// ব্যবহার
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MultiProvider(
      providers: [
        ChangeNotifierProvider(create: (_) => UserProfile()),
        ValueListenableProvider(create: (_) => Counter()),
      ],
      child: MaterialApp(
        home: Scaffold(
          body: Column(
            children: [
              // ChangeNotifier ব্যবহার
              Consumer<UserProfile>(
                builder: (context, user, child) {
                  return Text('Name: ${user.name}, Age: ${user.age}');
                },
              ),
              // ValueNotifier ব্যবহার
              ValueListenableBuilder<int>(
                valueListenable: context.read<Counter>(),
                builder: (context, value, child) {
                  return Text('Count: $value');
                },
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

**Pitfalls:**
1. `ChangeNotifier`-এ `notifyListeners()` ভুলে গেলে UI আপডেট হবে না।
2. `ValueNotifier`-এ কমপ্লেক্স অবজেক্ট রাখলে ডিপ কপি ইস্যু হতে পারে।
3. `ChangeNotifier`-এ অপ্রয়োজনীয় `notifyListeners()` কল করলে পারফরম্যান্স খারাপ হয়।

**Interview Tips:** 
- Simple counter/flag → `ValueNotifier`
- Feature state with multiple fields → `ChangeNotifier`
- `ValueNotifier` বেশি পারফরম্যান্স কারণ এটি শুধুমাত্র একটি ভ্যালু ট্র্যাক করে।

---

### প্রশ্ন ৫: Provider-এ পারফরম্যান্স অপ্টিমাইজ কিভাবে করবেন?

**উত্তর (ডিটেইল):**

Provider-এ পারফরম্যান্স অপ্টিমাইজেশন একটি গুরুত্বপূর্ণ বিষয়, বিশেষত বড় অ্যাপ্লিকেশনে। এখানে কয়েকটি কার্যকর কৌশল:

**১. উইজেট স্প্লিটিং (Widget Splitting)**:
- বড় উইজেটকে ছোট ছোট অংশে ভাগ করুন
- শুধুমাত্র যে অংশ স্টেট শোনে সেই অংশটাই রিবিল্ড করান
- অপ্রয়োজনীয় রিবিল্ড এড়ান

```dart
// খারাপ উদাহরণ - পুরো উইজেট রিবিল্ড হয়
class BadExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final user = context.watch<User>();
    
    return Column(
      children: [
        Text('Name: ${user.name}'), // নাম পরিবর্তন হলে পুরো Column রিবিল্ড হবে
        Text('Age: ${user.age}'),   // বয়স পরিবর্তন হলেও পুরো Column রিবিল্ড হবে
        // অনেক বেশি UI elements...
      ],
    );
  }
}

// ভালো উদাহরণ - শুধুমাত্র প্রয়োজনীয় অংশ রিবিল্ড হয়
class GoodExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        UserNameWidget(), // শুধুমাত্র নাম পরিবর্তন হলে এই উইজেট রিবিল্ড হবে
        UserAgeWidget(),  // শুধুমাত্র বয়স পরিবর্তন হলে এই উইজেট রিবিল্ড হবে
        // অনেক বেশি UI elements...
      ],
    );
  }
}

class UserNameWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final userName = context.watch<User>().name;
    return Text('Name: $userName');
  }
}

class UserAgeWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final userAge = context.watch<User>().age;
    return Text('Age: $userAge');
  }
}
```

**২. `context.select` ব্যবহার**:
- শুধুমাত্র নির্দিষ্ট ফিল্ড পরিবর্তন হলে রিবিল্ড করুন
- অপ্রয়োজনীয় রিবিল্ড এড়ান

```dart
class OptimizedWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // শুধুমাত্র নাম পরিবর্তন হলে রিবিল্ড হবে
    final userName = context.select<User, String>((user) => user.name);
    
    // শুধুমাত্র বয়স পরিবর্তন হলে রিবিল্ড হবে
    final userAge = context.select<User, int>((user) => user.age);
    
    return Column(
      children: [
        Text('Name: $userName'),
        Text('Age: $userAge'),
      ],
    );
  }
}
```

**৩. `const` কনস্ট্রাক্টর ব্যবহার**:
- অপরিবর্তনীয় উইজেটগুলোতে `const` ব্যবহার করুন
- Flutter-এ `const` উইজেট একবার তৈরি হয় এবং পুনরায় ব্যবহার হয়

```dart
class OptimizedList extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final items = context.watch<ItemList>().items;
    
    return ListView.builder(
      itemCount: items.length,
      itemBuilder: (context, index) {
        return const ItemTile(); // const ব্যবহার
      },
    );
  }
}

class ItemTile extends StatelessWidget {
  const ItemTile({Key? key}) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return const Card(
      child: ListTile(
        title: Text('Item'),
        subtitle: Text('Description'),
      ),
    );
  }
}
```

**৪. `Consumer` ব্যবহার**:
- শুধুমাত্র নির্দিষ্ট অংশে স্টেট শুনুন
- পুরো উইজেট রিবিল্ড এড়ান

```dart
class ConsumerExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: const AppBar(title: Text('App')), // const - রিবিল্ড হবে না
      body: Column(
        children: [
          const StaticWidget(), // const - রিবিল্ড হবে না
          Consumer<User>(
            builder: (context, user, child) {
              // শুধুমাত্র User স্টেট পরিবর্তন হলে এই অংশ রিবিল্ড হবে
              return Text('User: ${user.name}');
            },
          ),
        ],
      ),
    );
  }
}
```

**৫. `Selector` উইজেট ব্যবহার**:
- আরও গ্রানুলার কন্ট্রোলের জন্য
- শুধুমাত্র নির্দিষ্ট শর্ত পূরণ হলে রিবিল্ড

```dart
class SelectorExample extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Selector<User, String>(
      selector: (context, user) => user.name,
      builder: (context, name, child) {
        return Text('Name: $name');
      },
    );
  }
}
```

**Pitfalls:**
1. `build` মেথডে কমপ্লেক্স লজিক রাখলে পারফরম্যান্স খারাপ হয়
2. অপ্রয়োজনীয় `context.watch` ব্যবহার করলে অতিরিক্ত রিবিল্ড হয়
3. `const` ব্যবহার না করলে অপ্রয়োজনীয় উইজেট তৈরি হয়

**Interview Tips:** 
- বড় লিস্টে প্রতি আইটেমে `Selector`/`Consumer` রাখলে জ্যাঙ্ক কমে
- `context.select` সবচেয়ে কার্যকর পারফরম্যান্স অপ্টিমাইজেশন
- `const` ব্যবহার করে মেমরি ব্যবহার কমান


