## ইন্টারভিউ প্রশ্ন ও উত্তর: Flutter Widgets (পর্ব ১০)

### প্রশ্ন ৯১: `AnimatedContainer` এবং `TweenAnimationBuilder`-এর মধ্যে পার্থক্য কী?

**উত্তর:**

`AnimatedContainer` হলো একটি ইম্প্লিসিট অ্যানিমেটেড উইজেট যা স্বয়ংক্রিয়ভাবে তার প্রপার্টি পরিবর্তনের সময় একটি মসৃণ অ্যানিমেশন তৈরি করে। আপনি এর `duration` এবং `curve` সেট করতে পারেন, এবং যখন এর যেকোনো অ্যানিমেটেবল প্রপার্টি (যেমন `width`, `height`, `color`, `padding`, ইত্যাদি) পরিবর্তিত হয়, তখন এটি স্বয়ংক্রিয়ভাবে পুরোনো মান থেকে নতুন মানে অ্যানিমেট করে।

উদাহরণ:

```
dart
AnimatedContainer(
  duration: Duration(seconds: 1),
  curve: Curves.easeInOut,
  width: _isExpanded ? 200.0 : 100.0,
  height: _isExpanded ? 200.0 : 100.0,
  color: _isExpanded ? Colors.blue : Colors.red,
  child: Center(child: Text('Hello')),
);
```
অন্যদিকে, `TweenAnimationBuilder` হলো একটি আরও সাধারণ এবং নমনীয় উইজেট যা যেকোনো প্রপার্টিতে অ্যানিমেশন তৈরি করতে পারে, শুধুমাত্র উইজেটের দৃশ্যমান প্রপার্টিতে নয়। এটি একটি `tween` গ্রহণ করে যা অ্যানিমেশনের শুরু এবং শেষের মান নির্ধারণ করে এবং একটি `builder` ফাংশন যা অ্যানিমেশনের বর্তমান মান ব্যবহার করে উইজেট তৈরি করে। এটি ইম্প্লিসিট নয়, অর্থাৎ আপনাকে স্পষ্টভাবে অ্যানিমেট করার জন্য মান পরিবর্তন করতে হবে।

উদাহরণ:
```
dart
TweenAnimationBuilder<double>(
  tween: Tween<double>(begin: 0, end: _sliderValue),
  duration: Duration(milliseconds: 500),
  builder: (BuildContext context, double size, Widget? child) {
    return Container(
      width: size * 100,
      height: size * 100,
      color: Colors.blue,
    );
  },
);
```
সংক্ষেপে, `AnimatedContainer` সহজ অ্যানিমেশনের জন্য সুবিধাজনক যেখানে আপনি একটি কনটেইনারের প্রপার্টি অ্যানিমেট করতে চান, আর `TweenAnimationBuilder` আরও কাস্টম এবং জটিল অ্যানিমেশনের জন্য ব্যবহৃত হয় যেখানে আপনি যেকোনো ডেটা টাইপ অ্যানিমেট করতে পারেন।

### প্রশ্ন ৯২: Flutter-এ `SafeArea` উইজেট কেন ব্যবহার করা হয়?

**উত্তর:**

`SafeArea` উইজেট ব্যবহার করা হয় আপনার UI-কে ডিভাইসের অপারেটিং সিস্টেমের দ্বারা প্রভাবিত অবাঞ্ছিত এলাকা থেকে রক্ষা করার জন্য। এই অবাঞ্ছিত এলাকাগুলির মধ্যে অন্তর্ভুক্ত থাকতে পারে নোচ (notch), স্ট্যাটাস বার, নেভিগেশন বার বা ডিভাইসের বেজেল। `SafeArea` উইজেট এই এলাকাগুলিকে সনাক্ত করে এবং আপনার উইজেটের চারপাশে পর্যাপ্ত প্যাডিং যোগ করে যাতে আপনার UI এই এলাকাগুলির নিচে চাপা না পড়ে বা তাদের দ্বারা আংশিকভাবে ঢেকে না যায়।

উদাহরণ:

```
dart
Scaffold(
  appBar: AppBar(title: Text('SafeArea Example')),
  body: SafeArea(
    child: Center(
      child: Text('This content is safe from system UI.'),
    ),
  ),
);
```
`SafeArea` ডিফল্টরূপে সমস্ত দিক থেকে প্যাডিং যোগ করে, তবে আপনি `top`, `bottom`, `left`, `right` প্রপার্টি ব্যবহার করে নির্দিষ্ট দিকগুলিতে প্যাডিং নিয়ন্ত্রণ করতে পারেন। এটি মোবাইল অ্যাপ্লিকেশন ডিজাইনে একটি অপরিহার্য উইজেট যা নিশ্চিত করে যে আপনার UI বিভিন্ন ডিভাইসে সঠিকভাবে প্রদর্শিত হয়।

### প্রশ্ন ৯৩: Flutter-এ `Flexible` এবং `Expanded` উইজেটের মধ্যে মূল পার্থক্য কী?

**উত্তর:**

`Flexible` এবং `Expanded` উভয়ই `Row`, `Column`, এবং `Flex` উইজেটের মধ্যে থাকা চাইল্ড উইজেটগুলির লেআউট নিয়ন্ত্রণ করতে ব্যবহৃত হয়।

**`Expanded`:**

* `Expanded` উইজেট তার প্যারেন্টের উপলব্ধ স্থান সম্পূর্ণরূপে দখল করে।
* এটি `FlexFit.tight`-এর সমতুল্য।
* এটি চাইল্ডকে প্রসারিত হতে বাধ্য করে।

উদাহরণ:
```
dart
Row(
  children: <Widget>[
    Container(color: Colors.red, width: 50),
    Expanded(
      child: Container(color: Colors.blue),
    ),
    Container(color: Colors.green, width: 50),
  ],
);
```
এই ক্ষেত্রে, নীল কনটেইনারটি লাল এবং সবুজ কনটেইনারের অবশিষ্ট সমস্ত স্থান দখল করবে।

**`Flexible`:**

* `Flexible` উইজেট তার প্যারেন্টের উপলব্ধ স্থানের মধ্যে তার চাইল্ডের জন্য স্থান বরাদ্দ করে, তবে এটি চাইল্ডকে প্রসারিত হতে বাধ্য করে না।
* এটি `FlexFit.loose`-এর সমতুল্য (ডিফল্ট)।
* আপনি `flex` প্রপার্টি ব্যবহার করে চাইল্ডগুলি কতটুকু উপলব্ধ স্থান ব্যবহার করবে তা নিয়ন্ত্রণ করতে পারেন।

উদাহরণ:

```
dart
Row(
  children: <Widget>[
    Container(color: Colors.red, width: 50),
    Flexible(
      flex: 2,
      child: Container(color: Colors.blue),
    ),
    Flexible(
      flex: 1,
      child: Container(color: Colors.green),
    ),
  ],
);
```
এখানে, নীল কনটেইনারটি সবুজ কনটেইনারের দ্বিগুণ স্থান দখল করবে (যদি উপলব্ধ স্থান থাকে)।

সংক্ষেপে, `Expanded` উপলব্ধ স্থান সম্পূর্ণরূপে পূরণ করার জন্য ব্যবহৃত হয়, যখন `Flexible` চাইল্ডের আকার নির্ধারণের জন্য আরও বেশি নমনীয়তা প্রদান করে এবং উপলব্ধ স্থানের একটি অংশ বিতরণ করতে ব্যবহৃত হয়।

### প্রশ্ন ৯৪: Flutter-এ `FutureBuilder` উইজেট কীভাবে কাজ করে এবং কখন এটি ব্যবহার করবেন?

**উত্তর:**

`FutureBuilder` উইজেট একটি `Future` এর সাথে ইন্টারঅ্যাক্ট করতে এবং `Future` সম্পন্ন হওয়ার সময় আপনার UI-কে রিবিল্ড করতে ব্যবহৃত হয়। এটি আপনাকে একটি `Future` এর বর্তমান অবস্থার (যেমন লোডিং, সম্পন্ন, ত্রুটি) উপর ভিত্তি করে বিভিন্ন উইজেট প্রদর্শন করার অনুমতি দেয়।

**এটি কীভাবে কাজ করে:**

1. আপনি একটি `Future` প্রদান করেন যা থেকে ডেটা লোড করা হবে।
2. আপনি একটি `builder` ফাংশন প্রদান করেন যা একটি `BuildContext` এবং একটি `AsyncSnapshot` গ্রহণ করে।
3. `AsyncSnapshot` এ `Future` এর বর্তমান অবস্থা এবং ডেটা (যদি সম্পন্ন হয়) বা ত্রুটি (যদি ত্রুটি হয়) থাকে।
4. `Future` এর অবস্থা পরিবর্তন হওয়ার সাথে সাথে `builder` ফাংশনটি পুনরায় কল করা হয়, যা আপনাকে আপনার UI আপডেট করতে দেয়।

**কখন এটি ব্যবহার করবেন:**

`FutureBuilder` ব্যবহার করবেন যখন আপনার UI-কে অ্যাসিঙ্ক্রোনাস ডেটা লোড হওয়ার জন্য অপেক্ষা করতে হবে, যেমন:

* নেটওয়ার্ক থেকে ডেটা আনা।
* ডেটাবেস থেকে ডেটা লোড করা।
* ফাইল থেকে ডেটা পড়া।
* অন্যান্য অ্যাসিঙ্ক্রোনাস অপারেশন সম্পাদন করা।

উদাহরণ:

```
dart
FutureBuilder<String>(
  future: _loadData(), // একটি Future যা স্ট্রিং রিটার্ন করে
  builder: (BuildContext context, AsyncSnapshot<String> snapshot) {
    if (snapshot.connectionState == ConnectionState.waiting) {
      return CircularProgressIndicator(); // লোডিং অবস্থায়
    } else if (snapshot.hasError) {
      return Text('Error: ${snapshot.error}'); // ত্রুটি অবস্থায়
    } else {
      return Text('Data: ${snapshot.data}'); // ডেটা লোড সম্পন্ন হলে
    }
  },
);

Future<String> _loadData() async {
  await Future.delayed(Duration(seconds: 2));
  return 'Loaded Data';
}
```
`FutureBuilder` অ্যাসিঙ্ক্রোনাস ডেটা হ্যান্ডলিং সহজ করে এবং আপনার UI কে ডেটা লোডিং প্রক্রিয়া চলাকালীন প্রতিক্রিয়াশীল রাখে।

### প্রশ্ন ৯৫: Flutter-এ `StreamBuilder` উইজেট কীভাবে কাজ করে এবং কখন এটি ব্যবহার করবেন?

**উত্তর:**

`StreamBuilder` উইজেট একটি `Stream` এর সাথে ইন্টারঅ্যাক্ট করতে এবং `Stream` থেকে নতুন ডেটা আসার সময় আপনার UI-কে রিবিল্ড করতে ব্যবহৃত হয়। এটি আপনাকে একটি `Stream` এর বর্তমান অবস্থা এবং সর্বশেষ ডেটার উপর ভিত্তি করে বিভিন্ন উইজেট প্রদর্শন করার অনুমতি দেয়।

**এটি কীভাবে কাজ করে:**

1. আপনি একটি `Stream` প্রদান করেন যা থেকে ডেটা শোনা হবে।
2. আপনি একটি `builder` ফাংশন প্রদান করেন যা একটি `BuildContext` এবং একটি `AsyncSnapshot` গ্রহণ করে।
3. `AsyncSnapshot` এ `Stream` এর বর্তমান অবস্থা এবং সর্বশেষ ডেটা থাকে।
4. `Stream` থেকে নতুন ডেটা আসার সাথে সাথে `builder` ফাংশনটি পুনরায় কল করা হয়, যা আপনাকে আপনার UI আপডেট করতে দেয়।

**কখন এটি ব্যবহার করবেন:**

`StreamBuilder` ব্যবহার করবেন যখন আপনার UI-কে সময়ের সাথে সাথে পরিবর্তনশীল ডেটা প্রদর্শন করতে হবে, যেমন:

* রিয়েল-টাইম ডেটা ফিড (যেমন চ্যাট মেসেজ)।
* ডেটাবেসের লাইভ আপডেট।
* সেন্সর ডেটা।
* অন্যান্য অ্যাসিঙ্ক্রোনাস ডেটা স্ট্রিম।

উদাহরণ:

```
dart
StreamBuilder<int>(
  stream: _counterStream(), // একটি Stream যা ইন্টিজার নির্গত করে
  builder: (BuildContext context, AsyncSnapshot<int> snapshot) {
    if (snapshot.connectionState == ConnectionState.waiting) {
      return Text('Waiting for data...'); // ডেটার জন্য অপেক্ষা করছে
    } else if (snapshot.hasError) {
      return Text('Error: ${snapshot.error}'); // ত্রুটি অবস্থায়
    } else {
      return Text('Count: ${snapshot.data}'); // সর্বশেষ ডেটা
    }
  },
);

Stream<int> _counterStream() async* {
  for (int i = 1; i <= 5; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}
```
`StreamBuilder` রিয়েল-টাইম ডেটা হ্যান্ডলিং সহজ করে এবং আপনার UI কে ডেটা স্ট্রিমের সাথে সামঞ্জস্যপূর্ণ রাখে।

### প্রশ্ন ৯৬: Flutter-এ `CustomScrollView` এবং `Slivers`-এর ধারণা ব্যাখ্যা করুন।

**উত্তর:**

Flutter-এ `CustomScrollView` হলো একটি স্ক্রোলিং উইজেট যা একাধিক স্ক্রোলযোগ্য উইজেট (Slivers নামে পরিচিত) কে একত্রিত করে একটি একক স্ক্রোল প্রভাব তৈরি করতে ব্যবহৃত হয়। ঐতিহ্যগত স্ক্রোলযোগ্য উইজেট (যেমন `ListView`, `GridView`) শুধুমাত্র এক ধরনের লেআউট প্রদর্শন করতে পারে, কিন্তু `CustomScrollView` আপনাকে বিভিন্ন ধরণের স্ক্রোলযোগ্য লেআউট এবং প্রভাবকে একত্রিত করার অনুমতি দেয়।

**Slivers:**

Slivers হলো `CustomScrollView`-এর চাইল্ড উইজেট। এগুলি হলো বিশেষ উইজেট যা স্ক্রোলিং এলাকাতে কতটুকু দৃশ্যমান হবে তা নিয়ন্ত্রণ করতে পারে। Slivers এর উদাহরণগুলির মধ্যে রয়েছে:

* `SliverAppBar`: একটি অ্যাপ বার যা স্ক্রোল করার সময় সঙ্কুচিত বা প্রসারিত হতে পারে।
* `SliverList`: একটি লিস্ট ভিউ যা স্ক্রোলযোগ্য।
* `SliverGrid`: একটি গ্রিড ভিউ যা স্ক্রোলযোগ্য।
* `SliverFillRemaining`: উপলব্ধ স্ক্রোলিং স্থান পূরণ করার জন্য ব্যবহৃত হয়।

`CustomScrollView` এবং Slivers ব্যবহার করে আপনি আরও জটিল এবং কাস্টমাইজড স্ক্রোলিং অভিজ্ঞতা তৈরি করতে পারেন যা সাধারণ `ListView` বা `GridView` দ্বারা সম্ভব নয়।

উদাহরণ:

```
dart
CustomScrollView(
  slivers: <Widget>[
    SliverAppBar(
      expandedHeight: 200.0,
      flexibleSpace: FlexibleSpaceBar(
        title: Text('Sliver AppBar'),
        background: Image.network(
          'https://via.placeholder.com/150',
          fit: BoxFit.cover,
        ),
      ),
    ),
    SliverList(
      delegate: SliverChildBuilderDelegate(
        (BuildContext context, int index) {
          return Container(
            height: 50.0,
            color: index % 2 == 0 ? Colors.white : Colors.grey[200],
            child: Center(child: Text('Item $index')),
          );
        },
        childCount: 20,
      ),
    ),
  ],
);
```
এই উদাহরণে, একটি `SliverAppBar` এবং একটি `SliverList` একটি `CustomScrollView`-এর মধ্যে একত্রিত হয়েছে।

### প্রশ্ন ৯৭: Flutter-এ `Key` কেন গুরুত্বপূর্ণ এবং কখন আপনি এগুলি ব্যবহার করবেন?

**উত্তর:**

Flutter-এ `Key` হলো একটি ঐচ্ছিক শনাক্তকারী যা উইজেট, এলিমেন্ট এবং স্টেট অবজেক্টগুলিকে অনন্যভাবে চিহ্নিত করতে ব্যবহৃত হয়। এগুলি Flutter ফ্রেমওয়ার্ককে রিবিল্ডের সময় উইজেট ট্রি ম্যানেজ করতে এবং দক্ষতার সাথে আপডেট করতে সাহায্য করে।

**কেন গুরুত্বপূর্ণ:**

যখন উইজেট ট্রি আপডেট হয়, Flutter ফ্রেমওয়ার্ক পুরানো উইজেটগুলির সাথে নতুন উইজেটগুলির তুলনা করে কোন উইজেটগুলি পরিবর্তন হয়েছে, যোগ করা হয়েছে বা সরানো হয়েছে তা নির্ধারণ করতে। যদি উইজেটগুলির মধ্যে `Key` থাকে, Flutter একই `Key` সহ উইজেটগুলিকে একই এলিমেন্ট হিসাবে বিবেচনা করতে পারে, এমনকি যদি উইজেটের টাইপ বা কনফিগারেশন পরিবর্তিত হয়। এটি ফ্রেমওয়ার্ককে দক্ষতার সাথে উইজেট ট্রি আপডেট করতে এবং অবাঞ্ছিত রিবিল্ড এড়াতে সাহায্য করে।

**কখন ব্যবহার করবেন:**

`Key` ব্যবহার করা বিশেষভাবে গুরুত্বপূর্ণ যখন আপনার লিস্টে এমন উইজেট থাকে যা পরিবর্তন হতে পারে (যেমন নতুন উইজেট যোগ করা, সরানো বা পুনর্বিন্যাস করা)। কিছু সাধারণ ব্যবহারের ক্ষেত্রে অন্তর্ভুক্ত:

* ডায়নামিক লিস্ট ভিউ (`ListView.builder`, `GridView.builder`) যেখানে আইটেমগুলি যোগ করা, সরানো বা পুনর্বিন্যাস করা হতে পারে।
* যখন আপনি উইজেট ট্রি-তে একই ধরণের উইজেটগুলি পরিবর্তন করেন।
* যখন আপনি একটি উইজেটের স্টেট বজায় রাখতে চান যখন তার পজিশন উইজেট ট্রি-তে পরিবর্তিত হয়।

`Key`-এর প্রকারভেদ:

* `ValueKey`: একটি নির্দিষ্ট মান (যেমন স্ট্রিং, পূর্ণসংখ্যা) ব্যবহার করে উইজেট সনাক্ত করে।
* `ObjectKey`: একটি অবজেক্ট ব্যবহার করে উইজেট সনাক্ত করে।
* `GlobalKey`: অ্যাপ্লিকেশন জুড়ে অনন্যভাবে একটি উইজেট সনাক্ত করে। এটি উইজেটের স্টেট বা রেন্ডারবক্স অ্যাক্সেস করতে ব্যবহৃত হয়।

উদাহরণ:

```
dart
ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index) {
    return Dismissible(
      key: ValueKey(items[index]), // গুরুত্বপূর্ণ: Key ব্যবহার করা হয়েছে
      onDismissed: (direction) {
        setState(() {
          items.removeAt(index);
        });
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('${items[index]} dismissed')),
        );
      },
      child: ListTile(title: Text(items[index])),
    );
  },
);
```
এই উদাহরণে, `Dismissible` উইজেটের জন্য `ValueKey` ব্যবহার করা হয়েছে যাতে যখন একটি আইটেম সরানো হয় তখন Flutter সঠিকভাবে অন্যান্য আইটেমগুলি আপডেট করতে পারে।

### প্রশ্ন ৯৮: Flutter-এ `GlobalKey` কীভাবে ব্যবহার করবেন এবং এর সুবিধা কী?

**উত্তর:**

`GlobalKey` হলো Flutter-এ একটি অনন্য কী যা অ্যাপ্লিকেশন জুড়ে একটি উইজেট, তার এলিমেন্ট বা স্টেটকে সনাক্ত করতে ব্যবহৃত হয়। এটি আপনাকে উইজেট ট্রি-তে যেকোনো জায়গা থেকে একটি নির্দিষ্ট উইজেট অ্যাক্সেস করার অনুমতি দেয়।

**কীভাবে ব্যবহার করবেন:**

1. আপনি একটি `GlobalKey` এর একটি ইনস্ট্যান্স তৈরি করুন।
2. আপনি যে উইজেটটি অ্যাক্সেস করতে চান তার `key` প্রপার্টিতে এই `GlobalKey` অ্যাসাইন করুন।
3. আপনি `globalKey.currentWidget`, `globalKey.currentElement`, বা `globalKey.currentState` ব্যবহার করে উইজেট, এলিমেন্ট বা স্টেট অ্যাক্সেস করতে পারেন।

উদাহরণ:

```
dart
final GlobalKey<ScaffoldState> _scaffoldKey = GlobalKey<ScaffoldState>();

Scaffold(
  key: _scaffoldKey,
  appBar: AppBar(title: Text('GlobalKey Example')),
  body: Center(
    child: ElevatedButton(
      onPressed: () {
        _scaffoldKey.currentState?.openDrawer(); // GlobalKey ব্যবহার করে Drawer খোলা
      },
      child: Text('Open Drawer'),
    ),
  ),
  drawer: Drawer(
    child: ListView(
      children: <Widget>[
        ListTile(title: Text('Menu Item 1')),
        ListTile(title: Text('Menu Item 2')),
      ],
    ),
  ),
);
```
এই উদাহরণে, আমরা একটি `GlobalKey<ScaffoldState>` ব্যবহার করে `Scaffold`-এর স্টেট অ্যাক্সেস করছি এবং প্রোগ্রাম্যাটিকভাবে Drawer খুলছি।

**সুবিধা:**

* উইজেট ট্রি-তে যেকোনো জায়গা থেকে একটি নির্দিষ্ট উইজেট বা তার স্টেট অ্যাক্সেস করা।
* উইজেটের পজিশন বা আকারের মতো রেন্ডারিং তথ্য অ্যাক্সেস করা।
* নেভিগেশন এবং ডায়ালগ প্রদর্শনের মতো কিছু ফ্রেমওয়ার্ক অপারেশন সম্পাদন করা।
* উইজেট ট্রি-তে গতিশীলভাবে উইজেট যোগ করা বা সরানোর সময় তাদের স্টেট বজায় রাখা।

মনে রাখবেন যে `GlobalKey` ব্যবহার করার সময় সতর্কতা অবলম্বন করা উচিত কারণ এটি উইজেট ট্রি-তে শক্তিশালী রেফারেন্স তৈরি করতে পারে এবং ভুলভাবে ব্যবহার করলে মেমরি লিক হতে পারে।

### প্রশ্ন ৯৯: Flutter-এ `InheritedWidget` কী এবং এটি স্টেট ম্যানেজমেন্টে কীভাবে সাহায্য করে?

**উত্তর:**

`InheritedWidget` হলো Flutter-এ একটি বিশেষ ধরণের উইজেট যা তার সাবট্রি-তে ডেটা সরবরাহ করতে ব্যবহৃত হয়। যখন একটি `InheritedWidget` এর ডেটা পরিবর্তিত হয়, তখন তার সাবট্রি-এর নির্ভরশিল উইজেটগুলি স্বয়ংক্রিয়ভাবে রিবিল্ড হয়।

**এটি কীভাবে স্টেট ম্যানেজমেন্টে সাহায্য করে:**

`InheritedWidget` আপনাকে উইজেট ট্রি-তে নিচে ডেটা পাস করার একটি কার্যকর উপায় সরবরাহ করে,Prop drilling (অনেক স্তরের নিচে ডেটা পাস করা) এড়িয়ে। একটি `InheritedWidget` এর ইনস্ট্যান্স উইজেট ট্রি-এর উপরে রাখুন এবং তার সাবট্রি-এর যেকোনো উইজেট `BuildContext.dependOnInheritedWidgetOfExactType<T>()` মেথড ব্যবহার করে ডেটা অ্যাক্সেস করতে পারে। যখন `InheritedWidget` এর ডেটা পরিবর্তিত হয়, `dependOnInheritedWidgetOfExactType` ব্যবহার করে ডেটা অ্যাক্সেসকারী সমস্ত উইজেট পুনরায় তৈরি হবে।

এটি ছোট থেকে মাঝারি আকারের অ্যাপ্লিকেশনগুলির জন্য একটি সরল স্টেট ম্যানেজমেন্ট সমাধান প্রদান করে। যদিও বড় অ্যাপ্লিকেশনগুলির জন্য Riverpod বা Provider-এর মতো শক্তিশালী স্টেট ম্যানেজমেন্ট সলিউশন ব্যবহার করার পরামর্শ দেওয়া হয়, যা `InheritedWidget`-এর উপর ভিত্তি করে তৈরি।

উদাহরণ:

```
dart
class MyInheritedWidget extends InheritedWidget {
  const MyInheritedWidget({
    Key? key,
    required this.data,
    required Widget child,
  }) : super(key: key, child: child);

  final String data;

  static MyInheritedWidget? of(BuildContext context) {
    return context.dependOnInheritedWidgetOfExactType<MyInheritedWidget>();
  }

  @override
  bool updateShouldNotify(MyInheritedWidget oldWidget) {
    return oldWidget.data != data;
  }
}

// InheritedWidget ব্যবহার করা একটি উইজেট
class MyTextWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final inheritedData = MyInheritedWidget.of(context)?.data ?? 'Default Data';
    return Text(inheritedData);
  }
}

// InheritedWidget ট্রি-এর উপরে রাখা
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MyInheritedWidget(
      data: 'Hello from InheritedWidget',
      child: MaterialApp(
        home: Scaffold(
          appBar: AppBar(title: Text('InheritedWidget Example')),
          body: Center(child: MyTextWidget()),
        ),
      ),
    );
  }
}
```
এই উদাহরণে, `MyInheritedWidget` তার `data` সাবট্রি-তে সরবরাহ করে এবং `MyTextWidget` সেই ডেটা অ্যাক্সেস করে। যখন `MyInheritedWidget` এর `data` পরিবর্তিত হয়, `MyTextWidget` স্বয়ংক্রিয়ভাবে রিবিল্ড হয়।

### প্রশ্ন ১০০: Flutter-এ `LayoutBuilder` উইজেট কী এবং কখন এটি ব্যবহার করবেন?

**উত্তর:**

`LayoutBuilder` উইজেট হলো একটি উইজেট যা আপনাকে প্যারেন্টের কনস্ট্রেইন্ট (constraints) উপর ভিত্তি করে একটি উইজেট ট্রি তৈরি করতে দেয়। এটি তার প্যারেন্টের আকার এবং সীমাবদ্ধতা সম্পর্কে তথ্য সরবরাহ করে, যা আপনাকে বর্তমান লেআউট অনুসারে আপনার UI কে গতিশীলভাবে মানিয়ে নিতে দেয়।

**এটি কীভাবে কাজ করে:**

1. আপনি `LayoutBuilder` উইজেট ব্যবহার করে একটি চাইল্ড তৈরি করেন।
2. আপনি একটি `builder` ফাংশন প্রদান করেন যা একটি `BuildContext` এবং একটি `BoxConstraints` অবজেক্ট গ্রহণ করে।
3. `BoxConstraints` অবজেক্টে প্যারেন্টের সর্বাধিক এবং সর্বনিম্ন প্রস্থ এবং উচ্চতার তথ্য থাকে।
4. আপনি এই কনস্ট্রেইন্ট ব্যবহার করে আপনার চাইল্ড উইজেটের লেআউট নির্ধারণ করতে পারেন।

**কখন ব্যবহার করবেন:**

`LayoutBuilder` ব্যবহার করবেন যখন আপনার উইজেটকে তার প্যারেন্টের উপলব্ধ স্থানের উপর ভিত্তি করে নিজেকে সামঞ্জস্য করতে হবে, যেমন:

* রেসপন্সিভ লেআউট তৈরি করা যা স্ক্রিনের আকারের উপর ভিত্তি করে পরিবর্তিত হয়।
* উইজেটের আকার তার প্যারেন্টের আকারের একটি শতাংশের উপর ভিত্তি করে নির্ধারণ করা।
* উপলব্ধ স্থান খুব কম হলে একটি ভিন্ন উইজেট প্রদর্শন করা।

উদাহরণ:

```
dart
Container(
  width: 300,
  height: 200,
  color: Colors.grey[300],
  child: LayoutBuilder(
    builder: (BuildContext context, BoxConstraints constraints) {
      if (constraints.maxWidth > 150) {
        return Center(child: Text('Wide Layout'));
      } else {
        return Center(child: Text('Narrow Layout'));
      }
    },
  ),
);
```
এই উদাহরণে, `LayoutBuilder` প্যারেন্টের প্রস্থ পরীক্ষা করে এবং প্রস্থ 150 এর বেশি হলে "Wide Layout" এবং অন্যথায় "Narrow Layout" প্রদর্শন করে। `LayoutBuilder` উইজেট আপনাকে আপনার UI-কে বিভিন্ন স্ক্রিন আকার এবং ওরিয়েন্টেশনের জন্য আরও নমনীয় এবং রেসপন্সিভ করতে সাহায্য করে।