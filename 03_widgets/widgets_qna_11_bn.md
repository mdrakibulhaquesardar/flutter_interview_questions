## Flutter Widgets Q&A (Bengali) - Part 11

**প্রশ্ন ১১২: Flutter এ `Flexible` এবং `Expanded` উইজেট দুটি কখন এবং কেন ব্যবহার করা হয়?**

**উত্তর:** `Flexible` এবং `Expanded` উইজেট দুটি সাধারণত `Row`, `Column`, অথবা `Flex` উইজেটের মধ্যে ব্যবহার করা হয়। এগুলি তাদের child উইজেটগুলিকে উপলব্ধ স্থান (available space) কিভাবে ব্যবহার করবে তা নিয়ন্ত্রণ করতে সাহায্য করে।

*   **`Expanded`:** এটি তার child উইজেটকে উপলব্ধ স্থান সম্পূর্ণরূপে ব্যবহার করতে বাধ্য করে। এটি একটি `Flexible` উইজেটের একটি বিশেষ রূপ যেখানে `flex` প্রোপার্টির ডিফল্ট মান ১ থাকে এবং `fit` প্রোপার্টির মান `FlexFit.tight` সেট করা থাকে।
    
```
dart
    Row(
      children: <Widget>[
        Container(color: Colors.red, width: 50),
        Expanded(
          child: Container(color: Colors.blue), // Available space will be filled by blue container
        ),
      ],
    )
    
```
*   **`Flexible`:** এটি তার child উইজেটকে উপলব্ধ স্থান ব্যবহার করার অনুমতি দেয়, কিন্তু তাকে সম্পূর্ণ স্থান ব্যবহার করতে বাধ্য করে না। এর `flex` প্রোপার্টির মান ১ বা তার বেশি হলে, child উইজেট তার flex factor অনুযায়ী উপলব্ধ স্থান ভাগ করে নেয়। `fit` প্রোপার্টির দুটি মান আছে:
    *   `FlexFit.tight`: child উইজেট উপলব্ধ স্থান সম্পূর্ণরূপে পূরণ করবে (এটি `Expanded` এর মত আচরণ করে)।
    *   `FlexFit.loose`: child উইজেট উপলব্ধ স্থানের মধ্যে তার নিজস্ব constraints অনুযায়ী স্থান নেবে, কিন্তু পূর্ণ স্থান ব্যবহার নাও করতে পারে।
```
dart
    Row(
      children: <Widget>[
        Container(color: Colors.red, width: 50),
        Flexible(
          flex: 2,
          child: Container(color: Colors.blue), // Takes 2 parts of available space
        ),
        Flexible(
          flex: 1,
          child: Container(color: Colors.green), // Takes 1 part of available space
        ),
      ],
    )
    
```
সংক্ষেপে, যখন আপনি চান একটি উইজেট উপলব্ধ স্থান সম্পূর্ণরূপে পূরণ করুক, তখন `Expanded` ব্যবহার করুন। যখন আপনি চান উইজেট উপলব্ধ স্থানের মধ্যে তার প্রয়োজন অনুযায়ী স্থান নিক এবং flex factor অনুযায়ী ভাগ করে নিক, তখন `Flexible` ব্যবহার করুন।

**প্রশ্ন ১১৩: Flutter এ `ListView.builder` কেন `ListView` এর চেয়ে বেশি পারফরম্যান্ট লম্বালম্বি তালিকা (long lists) প্রদর্শনের জন্য?**

**উত্তর:** লম্বালম্বি তালিকা বা প্রচুর সংখ্যক আইটেম প্রদর্শনের জন্য `ListView.builder` `ListView` এর চেয়ে বেশি পারফরম্যান্ট কারণ এটি "lazy loading" ব্যবহার করে।

*   **`ListView`:** যখন আপনি একটি সাধারণ `ListView` ব্যবহার করেন, তখন এটি তালিকার সমস্ত আইটেমগুলি একবারে তৈরি (render) করে, এমনকি যেগুলি স্ক্রিনে দেখা যাচ্ছে না সেগুলিও। এটি ছোট তালিকার জন্য ঠিক আছে, কিন্তু যখন তালিকার আকার অনেক বড় হয়, তখন এটি মেমরি এবং পারফরম্যান্সে নেতিবাচক প্রভাব ফেলতে পারে।

*   **`ListView.builder`:** এটি শুধুমাত্র সেই আইটেমগুলি তৈরি করে যেগুলি স্ক্রিনে বর্তমানে দৃশ্যমান বা কাছাকাছি রয়েছে। যখন ব্যবহারকারী স্ক্রোল করে, তখন এটি প্রয়োজন অনুযায়ী নতুন আইটেম তৈরি করে এবং স্ক্রিনের বাইরে চলে যাওয়া আইটেমগুলিকে ডিসপোজ করে। এটি মেমরির ব্যবহার অনেক কম রাখে এবং স্ক্রোলিংকে মসৃণ করে তোলে, বিশেষ করে বড় ডেটাসেটের জন্য।

তাই, যদি আপনার তালিকা ডেটা ডাইনামিক হয় বা আইটেমের সংখ্যা প্রচুর হয়, তবে `ListView.builder` ব্যবহার করা পারফরম্যান্সের জন্য অত্যন্ত গুরুত্বপূর্ণ।

**প্রশ্ন ১১৪: Flutter এ `Key` কি এবং কখন এটি ব্যবহার করা প্রয়োজন?**

**উত্তর:** Flutter এ `Key` হল একটি আইডেন্টিফায়ার যা ফ্রেমওয়ার্ককে উইজেট ট্রি (widget tree) এর মধ্যে উইজেটগুলির পরিচয় বজায় রাখতে সাহায্য করে যখন উইজেট ট্রি রি-বিল্ড (rebuild) হয়। সহজ ভাষায়, এটি Flutter কে বুঝতে সাহায্য করে যে একটি নির্দিষ্ট উইজেট তার আগের বিল্ড থেকে একই উইজেট কিনা, নাকি এটি একটি নতুন উইজেট।

`Key` ব্যবহার করার প্রয়োজন হয় যখন আপনি একই ধরণের উইজেটগুলির একটি ডাইনামিক তালিকা নিয়ে কাজ করেন এবং সেই উইজেটগুলির অবস্থা (state) বা ক্রম (order) পরিবর্তিত হতে পারে। উদাহরণস্বরূপ:

*   **ডাইনামিক তালিকা (Dynamic Lists):** যখন আপনি একটি তালিকা থেকে আইটেম যোগ, অপসারণ বা সর্ট করেন, তখন `Key` ব্যবহার না করলে Flutter ভুল উইজেটের সাথে ডেটা যুক্ত করতে পারে, যার ফলে ভুল উইজেট আপডেট হতে পারে বা ত্রুটি ঘটতে পারে।
    
```
dart
    // Example with keys for a dynamic list
    List<Widget> items = listOfStrings.map((String item) =>
      ListTile(
        key: ValueKey(item), // Using ValueKey with a unique value
        title: Text(item),
      ),
    ).toList();
    
```
*   **একই ধরণের একাধিক উইজেট (Multiple Similar Widgets):** যখন আপনার UI তে একই ধরণের একাধিক উইজেট থাকে এবং আপনি তাদের মধ্যে ডেটা বা অবস্থা পরিবর্তন করেন, তখন `Key` Flutter কে সঠিক উইজেটটি ট্র্যাক করতে সাহায্য করে।

`Key` এর প্রকারভেদ:

*   `ValueKey`: একটি নির্দিষ্ট মান ব্যবহার করে উইজেটকে চিহ্নিত করে।
*   `ObjectKey`: দুটি উইজেট একই কিনা তা নির্ধারণ করতে object এর সমানতা (equality) ব্যবহার করে।
*   `UniqueKey`: একটি অনন্য (unique) পরিচয় প্রদান করে, যা শুধুমাত্র একবার ব্যবহারের জন্য উপযুক্ত।
*   `GlobalKey`: সম্পূর্ণ অ্যাপ্লিকেশনের মধ্যে একটি উইজেটকে অনন্যভাবে চিহ্নিত করতে পারে। এটি কিছু অ্যাডভান্সড পরিস্থিতিতে কাজে আসে, যেমন একটি উইজেটের আকার বা অবস্থান পরিমাপ করা।

সংক্ষেপে, যখন উইজেট ট্রি তে উইজেটগুলির পরিচয় বজায় রাখা গুরুত্বপূর্ণ হয়, বিশেষ করে ডাইনামিক ডেটা বা তালিকার সাথে কাজ করার সময়, তখন `Key` ব্যবহার করা অপরিহার্য।

**প্রশ্ন ১১৫: Flutter এ `Padding` এবং `Margin` এর মধ্যে পার্থক্য কি?**

**উত্তর:** `Padding` এবং `Margin` উভয়ই উইজেটের চারপাশে স্থান যোগ করতে ব্যবহৃত হয়, তবে তারা উইজেটের বর্ডার (border) সাপেক্ষে ভিন্নভাবে কাজ করে।

*   **`Padding`:** এটি একটি উইজেটের বর্ডারের *ভেতরে* স্থান যোগ করে। Padding উইজেটের content এবং তার বর্ডারের মধ্যে ফাঁকা স্থান তৈরি করে। Padding উইজেটের আকারের (size) অংশ।
```
dart
    Container(
      color: Colors.blue,
      padding: EdgeInsets.all(16.0), // Padding inside the container
      child: Text('Hello'),
    )
    
```
*   **`Margin`:** এটি একটি উইজেটের বর্ডারের *বাইরে* স্থান যোগ করে। Margin উইজেট এবং তার চারপাশের অন্যান্য উইজেটগুলির মধ্যে ফাঁকা স্থান তৈরি করে। Margin উইজেটের আকারের অংশ নয়, বরং এটি উইজেটের অবস্থানকে প্রভাবিত করে।

    
```
dart
    Container(
      color: Colors.blue,
      margin: EdgeInsets.all(16.0), // Margin outside the container
      child: Text('Hello'),
    )
    
```
সহজভাবে বলতে গেলে, `Padding` একটি উইজেটের ভেতরের দিক থেকে জায়গা ছাড়ে, আর `Margin` উইজেটের বাইরের দিক থেকে জায়গা ছাড়ে।

**প্রশ্ন ১১৬: Flutter এ `Stack` উইজেট কি এবং কখন এটি ব্যবহার করা হয়?**

**উত্তর:** `Stack` উইজেট একাধিক উইজেটকে একটির উপরে অন্যটি স্তূপীকৃত (stack) করার জন্য ব্যবহৃত হয়। এটি আপনাকে একটি উইজেটের উপরে অন্য উইজেট ওভারলে (overlay) করতে দেয়। `Stack` উইজেটের children পজিশন করা যেতে পারে, যার ফলে আপনি তাদের অবস্থান নিয়ন্ত্রণ করতে পারেন।

`Stack` সাধারণত ব্যবহৃত হয় যখন আপনি:

*   একটি ব্যাকগ্রাউন্ড ইমেজের উপরে টেক্সট বা অন্য উইজেট রাখতে চান।
*   একটি আইকনের উপরে একটি বিজ্ঞপ্তি ব্যাজ (notification badge) যোগ করতে চান।
*   জটিল UI লেআউট তৈরি করতে চান যেখানে উপাদানগুলি একে অপরের উপর ওভারল্যাপ করে।

`Stack` এর children একটির উপরে একটি সাজানো হয়, লিস্টে যে উইজেট আগে থাকে সেটি নিচে থাকে এবং যে উইজেট পরে থাকে সেটি উপরে থাকে।
```
dart
Stack(
  children: <Widget>[
    Container(
      color: Colors.red,
      height: 200,
      width: 200,
    ),
    Positioned( // Used to position children within a Stack
      top: 50,
      left: 50,
      child: Container(
        color: Colors.blue,
        height: 100,
        width: 100,
      ),
    ),
    Align( // Used to align children within a Stack
      alignment: Alignment.bottomRight,
      child: Text(
        'Overlay Text',
        style: TextStyle(color: Colors.white),
      ),
    ),
  ],
)
```
`Positioned` এবং `Align` উইজেটগুলি `Stack` এর children এর অবস্থান নিয়ন্ত্রণ করতে ব্যবহৃত হয়।

**প্রশ্ন ১১৭: Flutter এ `SizedBox` উইজেট কি এবং এর ব্যবহার কি?**

**উত্তর:** `SizedBox` উইজেট একটি নির্দিষ্ট আকারের ফাঁকা স্থান তৈরি করতে বা তার child উইজেটকে একটি নির্দিষ্ট আকার দিতে ব্যবহৃত হয়। এটি একটি খুব সাধারণ এবং দরকারী উইজেট।

এর প্রধান ব্যবহারগুলি হলো:

*   **নির্দিষ্ট আকারের ফাঁকা স্থান তৈরি করা:** `SizedBox` এর `width` এবং `height` প্রোপার্টি ব্যবহার করে আপনি UI তে নির্দিষ্ট আকারের ফাঁকা স্থান তৈরি করতে পারেন। এটি `Padding` বা `Container` ব্যবহার না করে দুটি উইজেটের মধ্যে স্থান যোগ করার একটি সহজ উপায়।
    
```
dart
    Row(
      children: <Widget>[
        Container(color: Colors.red, width: 50),
        SizedBox(width: 20), // Adds 20 logical pixels of space
        Container(color: Colors.blue, width: 50),
      ],
    )
    
```
*   **Child উইজেটকে একটি নির্দিষ্ট আকার দেওয়া:** আপনি `SizedBox` এর child প্রোপার্টি ব্যবহার করে একটি উইজেটকে নির্দিষ্ট `width` এবং `height` দিতে পারেন।
```
dart
    SizedBox(
      width: 100,
      height: 50,
      child: RaisedButton(
        onPressed: () {},
        child: Text('Click Me'),
      ),
    )
    
```
*   **Child উইজেটকে তার আকার উপেক্ষা করতে বাধ্য করা:** যদি আপনি `SizedBox` এর `width` বা `height` কে `double.infinity` তে সেট করেন এবং এর child থাকে, তবে child তার parent এর constraint উপেক্ষা করে সেই নির্দিষ্ট দিকে উপলব্ধ সম্পূর্ণ স্থান ব্যবহার করবে।

`SizedBox` হলো UI তে ফাঁকা স্থান পরিচালনা করার জন্য একটি হালকা ওজনের এবং দক্ষ উইজেট।

**প্রশ্ন ১১৮: Flutter এ `AspectRatio` উইজেট কি এবং কিভাবে এটি কাজ করে?**

**উত্তর:** `AspectRatio` উইজেট তার child উইজেটকে একটি নির্দিষ্ট প্রস্থ-উচ্চতা অনুপাত (width-to-height ratio) বজায় রাখতে বাধ্য করে। এটি উইজেটটিকে তার parent এর constraint এর মধ্যে যতটা সম্ভব বড় করে তোলে, একই সাথে নির্দিষ্ট অনুপাত বজায় রাখে।

`AspectRatio` এর প্রধান প্রোপার্টি হলো `aspectRatio`, যা একটি `double` মান নেয়। এই মানটি প্রস্থ (width) কে উচ্চতা (height) দিয়ে ভাগ করে পাওয়া যায়। উদাহরণস্বরূপ, `aspectRatio: 2.0` মানে উইজেটের প্রস্থ তার উচ্চতার দ্বিগুণ হবে।

```
dart
Container(
  color: Colors.grey,
  child: AspectRatio(
    aspectRatio: 16 / 9, // Common aspect ratio for videos
    child: Image.network(
      'https://via.placeholder.com/150',
      fit: BoxFit.cover,
    ),
  ),
)
```
এই উদাহরণে, ইমেজ উইজেটটি `AspectRatio` উইজেটের constraint এর মধ্যে 16:9 অনুপাত বজায় রেখে প্রদর্শিত হবে। এটি সাধারণত ভিডিও প্লেয়ার বা ইমেজ প্রদর্শনের জন্য ব্যবহৃত হয় যেখানে একটি নির্দিষ্ট অনুপাত বজায় রাখা গুরুত্বপূর্ণ।

**প্রশ্ন ১১৯: Flutter এ `Flexible` উইজেটের `fit` প্রোপার্টি ব্যাখ্যা করো।**

**উত্তর:** `Flexible` উইজেটের `fit` প্রোপার্টি নির্ধারণ করে যে Flexible উইজেটের মধ্যে তার child উইজেট উপলব্ধ স্থান কিভাবে ব্যবহার করবে। এর দুটি সম্ভাব্য মান রয়েছে:

*   **`FlexFit.tight`:** যখন `fit` প্রোপার্টির মান `FlexFit.tight` সেট করা হয়, তখন Flexible উইজেটের child উপলব্ধ স্থান সম্পূর্ণরূপে পূরণ করতে বাধ্য হয়। এটি `Expanded` উইজেটের মতই আচরণ করে।
```
dart
    Row(
      children: <Widget>[
        Container(color: Colors.red, width: 50),
        Flexible(
          flex: 1,
          fit: FlexFit.tight, // Child will fill the available space
          child: Container(color: Colors.blue),
        ),
      ],
    )
    
```
*   **`FlexFit.loose`:** যখন `fit` প্রোপার্টির মান `FlexFit.loose` সেট করা হয়, তখন Flexible উইজেটের child উপলব্ধ স্থানের মধ্যে তার নিজস্ব constraints অনুযায়ী স্থান নিতে পারে। এটি উপলব্ধ স্থান সম্পূর্ণরূপে পূরণ করতে বাধ্য নয়।
```
dart
    Row(
      children: <Widget>[
        Container(color: Colors.red, width: 50),
        Flexible(
          flex: 1,
          fit: FlexFit.loose, // Child will take its own size within available space
          child: Container(color: Colors.blue, width: 200), // If space is available, it might take 200, otherwise less
        ),
      ],
    )
    
```
সংক্ষেপে, `FlexFit.tight` child কে উপলব্ধ স্থান পূরণ করতে বাধ্য করে, যখন `FlexFit.loose` child কে উপলব্ধ স্থানের মধ্যে তার নিজের আকার নিতে দেয়।

**প্রশ্ন ১২০: Flutter এ `Opacity` উইজেট কি এবং কিভাবে এটি ব্যবহার করা হয়?**

**উত্তর:** `Opacity` উইজেট তার child উইজেটের অস্বচ্ছতা (opacity) নিয়ন্ত্রণ করতে ব্যবহৃত হয়। এটি আপনাকে একটি উইজেটকে আংশিকভাবে স্বচ্ছ বা সম্পূর্ণ অদৃশ্য করে তুলতে দেয়।

`Opacity` উইজেটের প্রধান প্রোপার্টি হলো `opacity`, যা 0.0 থেকে 1.0 এর মধ্যে একটি `double` মান নেয়।

*   `0.0`: সম্পূর্ণ অদৃশ্য (fully transparent)।
*   `1.0`: সম্পূর্ণ অস্বচ্ছ (fully opaque)।
*   0.0 এবং 1.0 এর মধ্যে মানগুলি আংশিক স্বচ্ছতার জন্য ব্যবহৃত হয়।
```
dart
Opacity(
  opacity: 0.5, // Makes the child 50% transparent
  child: Container(
    color: Colors.red,
    height: 100,
    width: 100,
  ),
)
```
`Opacity` উইজেট অ্যানিমেশনের জন্যও খুব দরকারী। আপনি একটি `AnimatedOpacity` উইজেট ব্যবহার করে সময়ের সাথে সাথে অস্বচ্ছতা পরিবর্তন করতে পারেন।

**প্রশ্ন ১২১: Flutter এ `ShaderMask` উইজেট কি এবং কখন এটি ব্যবহার করা হয়?**

**উত্তর:** `ShaderMask` উইজেট তার child উইজেটকে একটি শেডার (shader) দিয়ে মাস্ক (mask) করতে ব্যবহৃত হয়। এটি আপনাকে child এর রেন্ডারিংয়ে বিভিন্ন ভিজ্যুয়াল এফেক্ট (visual effects) প্রয়োগ করতে দেয়। একটি শেডার হলো একটি প্রোগ্রাম যা নির্ধারণ করে কিভাবে প্রতিটি পিক্সেল রঙ করা হবে।

`ShaderMask` এর প্রধান প্রোপার্টিগুলি হলো:

*   **`shader`:** একটি `Shader` অবজেক্ট যা মাস্কিং এফেক্ট তৈরি করে। আপনি `LinearGradient`, `RadialGradient`, `SweepGradient` ইত্যাদি ব্যবহার করে শেডার তৈরি করতে পারেন।
*   **`blendMode`:** নির্ধারণ করে কিভাবে শেডার এবং child উইজেট মিশ্রিত (blend) হবে। বিভিন্ন `BlendMode` রয়েছে যা বিভিন্ন মিশ্রণ মোড প্রদান করে।

`ShaderMask` সাধারণত ব্যবহৃত হয় যখন আপনি:

*   টেক্সট বা ছবির উপর গ্রেডিয়েন্ট এফেক্ট প্রয়োগ করতে চান।
*   একটি নির্দিষ্ট আকার বা প্যাটার্ন দিয়ে একটি উইজেটের অংশ মাস্ক করতে চান।
*   জটিল ভিজ্যুয়াল এফেক্ট তৈরি করতে চান যা শুধুমাত্র রঙ পরিবর্তনের চেয়ে বেশি কিছু করে।

```
dart
ShaderMask(
  shaderCallback: (bounds) {
    return LinearGradient(
      colors: [Colors.red, Colors.blue],
      tileMode: TileMode.mirror,
    ).createShader(bounds);
  },
  blendMode: BlendMode.srcIn,
  child: Text(
    'Gradient Text',
    style: TextStyle(fontSize: 48, fontWeight: FontWeight.bold),
  ),
)
```
এই উদাহরণে, টেক্সট উইজেটটি একটি লাল থেকে নীল লিনিয়ার গ্রেডিয়েন্ট শেডার দিয়ে মাস্ক করা হয়েছে, যার ফলে টেক্সটের রঙ গ্রেডিয়েন্ট হয়ে গেছে। `ShaderMask` আপনাকে UI তে আরও সৃজনশীল এবং কাস্টম ভিজ্যুয়াল এফেক্ট যোগ করার ক্ষমতা দেয়।