### প্রশ্ন ৬: Riverpod কী এবং Provider থেকে আলাদা কীভাবে?

**উত্তর (ডিটেইল):**

**Riverpod** হলো Flutter-এর জন্য একটি আধুনিক এবং শক্তিশালী স্টেট ম্যানেজমেন্ট লাইব্রেরি যা Provider-এর উত্তরসূরি হিসেবে বিবেচিত হয়। এটি Provider-এর অনেক সমস্যা সমাধান করে এবং আরও শক্তিশালী ফিচার প্রদান করে।

**Riverpod-এর মূল বৈশিষ্ট্যগুলো:**

1. **Compile-time Safety**: Riverpod compile-time-এ টাইপ চেক করে, যা runtime-এ অনেক বাগ এড়াতে সাহায্য করে। Provider-এ অনেক সময় runtime-এ বাগ ধরা পড়ে, কিন্তু Riverpod-এ compile-time-এই সেগুলো ধরা পড়ে।

2. **BuildContext মুক্ত**: Riverpod-এ `BuildContext`-এর প্রয়োজন হয় না, যা কোডকে আরও টেস্টেবল এবং মডুলার করে তোলে। Provider-এ `context.watch()` বা `context.read()` ব্যবহার করতে হয়, কিন্তু Riverpod-এ `ref.watch()` বা `ref.read()` ব্যবহার করা হয়।

3. **Provider Overrides**: Riverpod-এ provider override করা খুব সহজ, যা টেস্টিং এবং ডেভেলপমেন্টে খুব উপকারী।

4. **Auto-dispose**: Riverpod স্বয়ংক্রিয়ভাবে provider-গুলো dispose করে যখন সেগুলোর আর প্রয়োজন হয় না, যা মেমরি লিক প্রতিরোধে সহায়তা করে।

5. **Family Modifiers**: Riverpod-এ `family` modifier ব্যবহার করে parameterized provider তৈরি করা যায়, যা Provider-এ সম্ভব নয়।

**Provider vs Riverpod তুলনা:**

| বৈশিষ্ট্য | Provider | Riverpod |
|-----------|----------|----------|
| Compile-time Safety | ❌ | ✅ |
| BuildContext প্রয়োজন | ✅ | ❌ |
| Provider Overrides | সীমিত | ✅ |
| Auto-dispose | ❌ | ✅ |
| Family Modifiers | ❌ | ✅ |
| Code Generation | ❌ | ✅ (optional) |

**উদাহরণ:**

```dart
// Provider উদাহরণ
class Counter extends ChangeNotifier {
  int _count = 0;
  int get count => _count;

  void increment() {
    _count++;
    notifyListeners();
  }
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (_) => Counter(),
      child: MaterialApp(
        home: Scaffold(
          body: Center(
            child: Column(
              children: [
                Consumer<Counter>(
                  builder: (context, counter, child) {
                    return Text('${counter.count}');
                  },
                ),
                ElevatedButton(
                  onPressed: () {
                    context.read<Counter>().increment();
                  },
                  child: Text('Increment'),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}

// Riverpod উদাহরণ
final counterProvider = StateNotifierProvider<CounterNotifier, int>((ref) {
  return CounterNotifier();
});

class CounterNotifier extends StateNotifier<int> {
  CounterNotifier() : super(0);

  void increment() {
    state++;
  }
}

class MyApp extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final count = ref.watch(counterProvider);
    
    return MaterialApp(
      home: Scaffold(
        body: Center(
          child: Column(
            children: [
              Text('$count'),
              ElevatedButton(
                onPressed: () {
                  ref.read(counterProvider.notifier).increment();
                },
                child: Text('Increment'),
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
1. Riverpod শেখার কার্ভ Provider-এর চেয়ে বেশি
2. Provider থেকে Riverpod-এ migration করতে সময় লাগে
3. Riverpod-এ code generation optional কিন্তু recommended

**Interview Tips:** 
- Riverpod modern এবং type-safe, কিন্তু Provider সহজ এবং শেখা সহজ
- বড় প্রজেক্টে Riverpod ভালো, ছোট প্রজেক্টে Provider যথেষ্ট
- Riverpod-এর auto-dispose feature মেমরি management-এ খুব উপকারী

---

### প্রশ্ন ৭: Riverpod-এর `Provider`, `StateProvider`, `StateNotifierProvider`, `FutureProvider`, `StreamProvider` কখন কোনটা?

**উত্তর (ডিটেইল):**

Riverpod-এ বিভিন্ন ধরনের provider রয়েছে, প্রতিটির আলাদা ব্যবহারের ক্ষেত্র আছে। এগুলো বুঝতে পারলে সঠিক provider বেছে নেওয়া সহজ হয়।

**১. `Provider<T>`**:
- **কাজ**: Read-only computed value প্রদান করে
- **কখন ব্যবহার করবেন**: যখন একটি value compute করতে হয় যা অন্য provider-এর উপর নির্ভর করে
- **উদাহরণ**: API response থেকে computed value, filtered list, formatted data

```dart
final userProvider = Provider<User>((ref) {
  return User(name: 'John', age: 25);
});

final userNameProvider = Provider<String>((ref) {
  final user = ref.watch(userProvider);
  return user.name.toUpperCase();
});
```

**২. `StateProvider<T>`**:
- **কাজ**: Simple mutable state প্রদান করে
- **কখন ব্যবহার করবেন**: যখন একটি simple value (string, int, bool) track করতে হয়
- **উদাহরণ**: Counter, toggle state, form field value

```dart
final counterProvider = StateProvider<int>((ref) => 0);

class CounterWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final count = ref.watch(counterProvider);
    
    return Column(
      children: [
        Text('Count: $count'),
        ElevatedButton(
          onPressed: () {
            ref.read(counterProvider.notifier).state++;
          },
          child: Text('Increment'),
        ),
      ],
    );
  }
}
```

**৩. `StateNotifierProvider<TNotifier, TState>`**:
- **কাজ**: Complex state machine প্রদান করে
- **কখন ব্যবহার করবেন**: যখন complex business logic এবং multiple actions প্রয়োজন হয়
- **উদাহরণ**: User authentication, shopping cart, todo list management

```dart
class TodoNotifier extends StateNotifier<List<Todo>> {
  TodoNotifier() : super([]);

  void addTodo(String title) {
    final todo = Todo(id: DateTime.now().toString(), title: title);
    state = [...state, todo];
  }

  void removeTodo(String id) {
    state = state.where((todo) => todo.id != id).toList();
  }

  void toggleTodo(String id) {
    state = state.map((todo) {
      if (todo.id == id) {
        return todo.copyWith(completed: !todo.completed);
      }
      return todo;
    }).toList();
  }
}

final todoProvider = StateNotifierProvider<TodoNotifier, List<Todo>>((ref) {
  return TodoNotifier();
});

class TodoWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final todos = ref.watch(todoProvider);
    
    return Column(
      children: [
        ...todos.map((todo) => ListTile(
          title: Text(todo.title),
          trailing: Checkbox(
            value: todo.completed,
            onChanged: (_) {
              ref.read(todoProvider.notifier).toggleTodo(todo.id);
            },
          ),
        )),
        ElevatedButton(
          onPressed: () {
            ref.read(todoProvider.notifier).addTodo('New Todo');
          },
          child: Text('Add Todo'),
        ),
      ],
    );
  }
}
```

**৪. `FutureProvider<T>`**:
- **কাজ**: Future থেকে আসা data handle করে
- **কখন ব্যবহার করবেন**: যখন API call, database query, বা async operation থেকে data load করতে হয়
- **উদাহরণ**: User profile loading, API data fetching

```dart
final userProfileProvider = FutureProvider<UserProfile>((ref) async {
  final response = await http.get(Uri.parse('https://api.example.com/user'));
  return UserProfile.fromJson(jsonDecode(response.body));
});

class UserProfileWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final userProfileAsync = ref.watch(userProfileProvider);
    
    return userProfileAsync.when(
      data: (userProfile) => Column(
        children: [
          Text('Name: ${userProfile.name}'),
          Text('Email: ${userProfile.email}'),
        ],
      ),
      loading: () => CircularProgressIndicator(),
      error: (error, stack) => Text('Error: $error'),
    );
  }
}
```

**৫. `StreamProvider<T>`**:
- **কাজ**: Stream থেকে আসা data handle করে
- **কখন ব্যবহার করবেন**: যখন real-time data, WebSocket, বা continuous data stream handle করতে হয়
- **উদাহরণ**: Real-time chat, live location updates, sensor data

```dart
final chatMessagesProvider = StreamProvider<List<Message>>((ref) {
  return Stream.periodic(Duration(seconds: 1), (_) {
    // Simulate real-time messages
    return List.generate(5, (index) => Message(
      id: index.toString(),
      text: 'Message $index',
      timestamp: DateTime.now(),
    ));
  });
});

class ChatWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final messagesAsync = ref.watch(chatMessagesProvider);
    
    return messagesAsync.when(
      data: (messages) => ListView.builder(
        itemCount: messages.length,
        itemBuilder: (context, index) {
          final message = messages[index];
          return ListTile(
            title: Text(message.text),
            subtitle: Text(message.timestamp.toString()),
          );
        },
      ),
      loading: () => CircularProgressIndicator(),
      error: (error, stack) => Text('Error: $error'),
    );
  }
}
```

**Provider Selection Guidelines:**

| Use Case | Provider Type | Example |
|----------|---------------|---------|
| Simple value | `StateProvider` | Counter, toggle |
| Computed value | `Provider` | Formatted text, filtered list |
| Complex state | `StateNotifierProvider` | User auth, shopping cart |
| Async data (one-time) | `FutureProvider` | API call, file read |
| Real-time data | `StreamProvider` | Chat, live updates |

**Pitfalls:**
1. `StateProvider` complex state-এর জন্য ব্যবহার করলে code maintain করা কঠিন হয়
2. `FutureProvider` real-time data-এর জন্য ব্যবহার করলে performance issue হয়
3. `StateNotifierProvider` simple value-এর জন্য overkill

**Interview Tips:** 
- Simple state → `StateProvider`
- Complex state → `StateNotifierProvider`
- Async data → `FutureProvider`/`StreamProvider`
- Computed value → `Provider`

---

### প্রশ্ন ৮: Riverpod `ref.watch`, `ref.read`, `ref.listen`—পার্থক্য?

**উত্তর (ডিটেইল):**

Riverpod-এ provider-এর সাথে interact করার জন্য তিনটি মূল method রয়েছে, প্রতিটির আলাদা উদ্দেশ্য এবং ব্যবহারের ক্ষেত্র আছে।

**১. `ref.watch(provider)`**:
- **কাজ**: একটি provider-কে subscribe করে এবং যখন সেই provider-এর state পরিবর্তিত হয় তখন widget-কে rebuild করে
- **কখন ব্যবহার করবেন**: যখন UI-তে state-এর পরিবর্তন দেখাতে হয়
- **সতর্কতা**: `build` method-এর বাইরে ব্যবহার করবেন না

```dart
class UserProfileWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // যখন userProvider-এর state পরিবর্তিত হয় তখন এই widget rebuild হবে
    final user = ref.watch(userProvider);
    
    return Column(
      children: [
        Text('Name: ${user.name}'),
        Text('Age: ${user.age}'),
      ],
    );
  }
}
```

**২. `ref.read(provider)`**:
- **কাজ**: একটি provider থেকে value বা method call করে, কিন্তু state পরিবর্তন হলে rebuild করে না
- **কখন ব্যবহার করবেন**: Event handler, callback, বা `build` method-এর বাইরে যখন শুধুমাত্র method call করতে হয়
- **সুবিধা**: Performance ভালো কারণ এটি rebuild trigger করে না

```dart
class LoginWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    return ElevatedButton(
      onPressed: () {
        // rebuild করে না, শুধুমাত্র method call করে
        ref.read(authProvider.notifier).login('user@example.com', 'password');
      },
      child: Text('Login'),
    );
  }
}
```

**৩. `ref.listen(provider, listener)`**:
- **কাজ**: একটি provider-এর state পরিবর্তন শুনে side effect trigger করে, কিন্তু UI rebuild করে না
- **কখন ব্যবহার করবেন**: যখন state পরিবর্তন হলে navigation, snackbar, dialog, বা অন্য side effect trigger করতে হয়
- **সতর্কতা**: Listener-এ infinite loop বা reaction chain তৈরি করবেন না

```dart
class AuthWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // যখন auth state পরিবর্তিত হয় তখন listener trigger হবে
    ref.listen<AuthState>(authProvider, (previous, next) {
      if (next is AuthStateAuthenticated) {
        // User logged in, navigate to home
        Navigator.of(context).pushReplacementNamed('/home');
      } else if (next is AuthStateUnauthenticated) {
        // User logged out, show login form
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Please log in')),
        );
      }
    });
    
    final authState = ref.watch(authProvider);
    
    return authState.when(
      authenticated: (user) => Text('Welcome ${user.name}'),
      unauthenticated: () => LoginForm(),
      loading: () => CircularProgressIndicator(),
    );
  }
}
```

**Advanced Usage Examples:**

**১. Conditional Watching:**
```dart
class ConditionalWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final isLoggedIn = ref.watch(authProvider).isAuthenticated;
    
    if (isLoggedIn) {
      // শুধুমাত্র logged in থাকলে user profile watch করবে
      final user = ref.watch(userProfileProvider);
      return UserProfileWidget(user: user);
    } else {
      return LoginWidget();
    }
  }
}
```

**২. Multiple Provider Watching:**
```dart
class DashboardWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // একসাথে multiple provider watch করা
    final user = ref.watch(userProvider);
    final notifications = ref.watch(notificationsProvider);
    final settings = ref.watch(settingsProvider);
    
    return Column(
      children: [
        Text('Welcome ${user.name}'),
        Text('You have ${notifications.length} notifications'),
        Switch(
          value: settings.darkMode,
          onChanged: (value) {
            ref.read(settingsProvider.notifier).toggleDarkMode();
          },
        ),
      ],
    );
  }
}
```

**৩. Selective Listening:**
```dart
class NotificationWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // শুধুমাত্র notification count পরিবর্তন হলে listen করবে
    ref.listen<int>(notificationCountProvider, (previous, next) {
      if (next > (previous ?? 0)) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('You have $next new notifications')),
        );
      }
    });
    
    final count = ref.watch(notificationCountProvider);
    return Badge(
      label: Text('$count'),
      child: Icon(Icons.notifications),
    );
  }
}
```

**Performance Optimization:**

**১. Selective Watching:**
```dart
class OptimizedWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // শুধুমাত্র প্রয়োজনীয় field watch করা
    final userName = ref.watch(userProvider.select((user) => user.name));
    final userAge = ref.watch(userProvider.select((user) => user.age));
    
    return Column(
      children: [
        Text('Name: $userName'), // শুধুমাত্র নাম পরিবর্তন হলে rebuild হবে
        Text('Age: $userAge'),   // শুধুমাত্র বয়স পরিবর্তন হলে rebuild হবে
      ],
    );
  }
}
```

**২. Computed Provider:**
```dart
final userDisplayNameProvider = Provider<String>((ref) {
  final user = ref.watch(userProvider);
  return '${user.firstName} ${user.lastName}';
});

class UserDisplayWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // শুধুমাত্র display name watch করা
    final displayName = ref.watch(userDisplayNameProvider);
    return Text(displayName);
  }
}
```

**Pitfalls:**
1. `build` method-এর বাইরে `watch` ব্যবহার করলে infinite loop হতে পারে
2. `listen`-এ complex logic রাখলে performance issue হতে পারে
3. `read` ব্যবহার করে state পরিবর্তন করলে UI update হবে না

**Interview Tips:** 
- `watch` → UI update-এর জন্য
- `read` → Event handler/callback-এর জন্য
- `listen` → Side effect trigger-এর জন্য
- `select` → Performance optimization-এর জন্য

---

### প্রশ্ন ৯: Riverpod `autoDispose` কেন দরকার?

**উত্তর (ডিটেইল):**

**`autoDispose`** হলো Riverpod-এর একটি গুরুত্বপূর্ণ feature যা memory management এবং resource optimization-এ সাহায্য করে। এটি provider-গুলোকে automatically dispose করে যখন সেগুলোর আর প্রয়োজন হয় না।

**`autoDispose` কেন দরকার:**

**১. Memory Management:**
- Provider-গুলো memory-তে জমা থাকে যতক্ষণ না সেগুলো manually dispose করা হয়
- `autoDispose` ব্যবহার করলে provider-গুলো automatically dispose হয় যখন কোনো listener থাকে না
- এটি memory leak প্রতিরোধে সাহায্য করে

**২. Resource Optimization:**
- Database connection, HTTP client, file stream ইত্যাদি resource-গুলো automatically close হয়
- Unused provider-গুলো memory থেকে মুছে যায়
- App performance উন্নত হয়

**৩. Lifecycle Management:**
- Screen change, navigation, বা widget disposal-এর সাথে সাথে provider-গুলো dispose হয়
- Temporary state (যেমন search result, form data) automatically clear হয়
- State persistence control করা যায়

**`autoDispose` ব্যবহারের উদাহরণ:**

**১. Basic Usage:**
```dart
// autoDispose ছাড়া - provider সবসময় memory-তে থাকবে
final userProvider = StateNotifierProvider<UserNotifier, User>((ref) {
  return UserNotifier();
});

// autoDispose সহ - listener না থাকলে automatically dispose হবে
final userProvider = StateNotifierProvider.autoDispose<UserNotifier, User>((ref) {
  return UserNotifier();
});
```

**২. Search Results:**
```dart
final searchResultsProvider = FutureProvider.autoDispose.family<List<Product>, String>((ref, query) async {
  if (query.isEmpty) return [];
  
  final response = await http.get(
    Uri.parse('https://api.example.com/search?q=$query'),
  );
  
  final data = jsonDecode(response.body) as List;
  return data.map((json) => Product.fromJson(json)).toList();
});

class SearchWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final query = ref.watch(searchQueryProvider);
    final searchResultsAsync = ref.watch(searchResultsProvider(query));
    
    return searchResultsAsync.when(
      data: (products) => ListView.builder(
        itemCount: products.length,
        itemBuilder: (context, index) => ProductTile(product: products[index]),
      ),
      loading: () => CircularProgressIndicator(),
      error: (error, stack) => Text('Error: $error'),
    );
  }
}
```

**৩. Form Data:**
```dart
final formDataProvider = StateNotifierProvider.autoDispose<FormNotifier, FormData>((ref) {
  return FormNotifier();
});

class FormNotifier extends StateNotifier<FormData> {
  FormNotifier() : super(FormData());
  
  void updateName(String name) {
    state = state.copyWith(name: name);
  }
  
  void updateEmail(String email) {
    state = state.copyWith(email: email);
  }
  
  void reset() {
    state = FormData();
  }
}

class FormWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final formData = ref.watch(formDataProvider);
    
    return Form(
      child: Column(
        children: [
          TextFormField(
            value: formData.name,
            onChanged: (value) {
              ref.read(formDataProvider.notifier).updateName(value);
            },
            decoration: InputDecoration(labelText: 'Name'),
          ),
          TextFormField(
            value: formData.email,
            onChanged: (value) {
              ref.read(formDataProvider.notifier).updateEmail(value);
            },
            decoration: InputDecoration(labelText: 'Email'),
          ),
          ElevatedButton(
            onPressed: () {
              ref.read(formDataProvider.notifier).reset();
            },
            child: Text('Reset'),
          ),
        ],
      ),
    );
  }
}
```

**৪. Cached Data with `keepAlive()`:**
```dart
final cachedUserProvider = FutureProvider.autoDispose<User>((ref) async {
  // Cache user data for 5 minutes
  ref.keepAlive();
  
  final response = await http.get(Uri.parse('https://api.example.com/user'));
  return User.fromJson(jsonDecode(response.body));
});

class UserProfileWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final userAsync = ref.watch(cachedUserProvider);
    
    return userAsync.when(
      data: (user) => Column(
        children: [
          Text('Name: ${user.name}'),
          Text('Email: ${user.email}'),
        ],
      ),
      loading: () => CircularProgressIndicator(),
      error: (error, stack) => Text('Error: $error'),
    );
  }
}
```

**৫. Debounced Search:**
```dart
final debouncedSearchProvider = FutureProvider.autoDispose.family<List<Product>, String>((ref, query) async {
  // Debounce search for 500ms
  await Future.delayed(Duration(milliseconds: 500));
  
  if (query.isEmpty) return [];
  
  final response = await http.get(
    Uri.parse('https://api.example.com/search?q=$query'),
  );
  
  final data = jsonDecode(response.body) as List;
  return data.map((json) => Product.fromJson(json)).toList();
});

class SearchWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final query = ref.watch(searchQueryProvider);
    final searchResultsAsync = ref.watch(debouncedSearchProvider(query));
    
    return Column(
      children: [
        TextField(
          onChanged: (value) {
            ref.read(searchQueryProvider.notifier).state = value;
          },
          decoration: InputDecoration(labelText: 'Search'),
        ),
        Expanded(
          child: searchResultsAsync.when(
            data: (products) => ListView.builder(
              itemCount: products.length,
              itemBuilder: (context, index) => ProductTile(product: products[index]),
            ),
            loading: () => CircularProgressIndicator(),
            error: (error, stack) => Text('Error: $error'),
          ),
        ),
      ],
    );
  }
}
```

**`autoDispose` vs `keepAlive()`:**

```dart
// autoDispose - listener না থাকলে dispose হবে
final temporaryDataProvider = StateProvider.autoDispose<String>((ref) => '');

// keepAlive - manually dispose না করা পর্যন্ত থাকবে
final persistentDataProvider = StateProvider.autoDispose<String>((ref) {
  ref.keepAlive();
  return '';
});
```

**Pitfalls:**
1. `autoDispose` ব্যবহার করলে state persistence নষ্ট হয়
2. `keepAlive()` overuse করলে memory leak হতে পারে
3. `autoDispose` provider-এ `ref.listen` ব্যবহার করলে dispose delay হতে পারে

**Interview Tips:** 
- Temporary state → `autoDispose`
- Persistent state → `keepAlive()` বা regular provider
- Search results, form data, temporary UI state → `autoDispose`
- User profile, app settings → regular provider

---

### প্রশ্ন ১০: Riverpod `family` modifier কবে ব্যবহার করবেন?

**উত্তর (ডিটেইল):**

**`family` modifier** হলো Riverpod-এর একটি শক্তিশালী feature যা parameterized provider তৈরি করতে সাহায্য করে। এটি একই provider-কে বিভিন্ন parameter-এর সাথে ব্যবহার করার সুযোগ দেয়।

**`family` modifier কেন দরকার:**

**১. Parameterized Providers:**
- একই provider-কে বিভিন্ন parameter-এর সাথে ব্যবহার করা যায়
- Instance-based caching এবং differentiation সহজ হয়
- Code reusability বাড়ে

**২. Scoped Lifecycle:**
- প্রতিটি parameter-এর জন্য আলাদা provider instance তৈরি হয়
- Parameter change হলে নতুন instance তৈরি হয়
- Memory management সহজ হয়

**৩. Type Safety:**
- Compile-time-এ parameter type check হয়
- Runtime error কম হয়
- Better IDE support

**`family` modifier ব্যবহারের উদাহরণ:**

**১. User Profile by ID:**
```dart
final userProvider = FutureProvider.family<User, String>((ref, userId) async {
  final response = await http.get(
    Uri.parse('https://api.example.com/users/$userId'),
  );
  
  return User.fromJson(jsonDecode(response.body));
});

class UserProfileWidget extends ConsumerWidget {
  final String userId;
  
  const UserProfileWidget({required this.userId, Key? key}) : super(key: key);
  
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final userAsync = ref.watch(userProvider(userId));
    
    return userAsync.when(
      data: (user) => Column(
        children: [
          Text('Name: ${user.name}'),
          Text('Email: ${user.email}'),
          Text('Age: ${user.age}'),
        ],
      ),
      loading: () => CircularProgressIndicator(),
      error: (error, stack) => Text('Error: $error'),
    );
  }
}

// ব্যবহার
class UserListWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    return ListView.builder(
      itemCount: userIds.length,
      itemBuilder: (context, index) {
        final userId = userIds[index];
        return UserProfileWidget(userId: userId);
      },
    );
  }
}
```

**২. Product Details by ID:**
```dart
final productProvider = FutureProvider.family<Product, String>((ref, productId) async {
  final response = await http.get(
    Uri.parse('https://api.example.com/products/$productId'),
  );
  
  return Product.fromJson(jsonDecode(response.body));
});

final productReviewsProvider = FutureProvider.family<List<Review>, String>((ref, productId) async {
  final response = await http.get(
    Uri.parse('https://api.example.com/products/$productId/reviews'),
  );
  
  final data = jsonDecode(response.body) as List;
  return data.map((json) => Review.fromJson(json)).toList();
});

class ProductDetailWidget extends ConsumerWidget {
  final String productId;
  
  const ProductDetailWidget({required this.productId, Key? key}) : super(key: key);
  
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final productAsync = ref.watch(productProvider(productId));
    final reviewsAsync = ref.watch(productReviewsProvider(productId));
    
    return productAsync.when(
      data: (product) => Column(
        children: [
          Text('Name: ${product.name}'),
          Text('Price: \$${product.price}'),
          Text('Description: ${product.description}'),
          reviewsAsync.when(
            data: (reviews) => Column(
              children: reviews.map((review) => ReviewWidget(review: review)).toList(),
            ),
            loading: () => CircularProgressIndicator(),
            error: (error, stack) => Text('Error loading reviews: $error'),
          ),
        ],
      ),
      loading: () => CircularProgressIndicator(),
      error: (error, stack) => Text('Error: $error'),
    );
  }
}
```

**৩. Search Results by Query:**
```dart
final searchResultsProvider = FutureProvider.family<List<Product>, String>((ref, query) async {
  if (query.isEmpty) return [];
  
  final response = await http.get(
    Uri.parse('https://api.example.com/search?q=${Uri.encodeComponent(query)}'),
  );
  
  final data = jsonDecode(response.body) as List;
  return data.map((json) => Product.fromJson(json)).toList();
});

class SearchWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final query = ref.watch(searchQueryProvider);
    final searchResultsAsync = ref.watch(searchResultsProvider(query));
    
    return Column(
      children: [
        TextField(
          onChanged: (value) {
            ref.read(searchQueryProvider.notifier).state = value;
          },
          decoration: InputDecoration(labelText: 'Search products'),
        ),
        Expanded(
          child: searchResultsAsync.when(
            data: (products) => ListView.builder(
              itemCount: products.length,
              itemBuilder: (context, index) => ProductTile(product: products[index]),
            ),
            loading: () => CircularProgressIndicator(),
            error: (error, stack) => Text('Error: $error'),
          ),
        ),
      ],
    );
  }
}
```

**৪. Multiple Parameters:**
```dart
// Multiple parameters with family
final filteredProductsProvider = FutureProvider.family<List<Product>, FilterParams>((ref, params) async {
  final response = await http.get(
    Uri.parse('https://api.example.com/products?category=${params.category}&price=${params.maxPrice}&sort=${params.sortBy}'),
  );
  
  final data = jsonDecode(response.body) as List;
  return data.map((json) => Product.fromJson(json)).toList();
});

class FilterParams {
  final String category;
  final double maxPrice;
  final String sortBy;
  
  const FilterParams({
    required this.category,
    required this.maxPrice,
    required this.sortBy,
  });
  
  @override
  bool operator ==(Object other) =>
      identical(this, other) ||
      other is FilterParams &&
          runtimeType == other.runtimeType &&
          category == other.category &&
          maxPrice == other.maxPrice &&
          sortBy == other.sortBy;
  
  @override
  int get hashCode => category.hashCode ^ maxPrice.hashCode ^ sortBy.hashCode;
}

class ProductListWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final filterParams = ref.watch(filterParamsProvider);
    final productsAsync = ref.watch(filteredProductsProvider(filterParams));
    
    return productsAsync.when(
      data: (products) => ListView.builder(
        itemCount: products.length,
        itemBuilder: (context, index) => ProductTile(product: products[index]),
      ),
      loading: () => CircularProgressIndicator(),
      error: (error, stack) => Text('Error: $error'),
    );
  }
}
```

**৫. Auto-dispose with Family:**
```dart
// autoDispose + family = per-parameter scoped lifecycle
final temporaryDataProvider = StateProvider.autoDispose.family<String, String>((ref, key) => '');

class TemporaryDataWidget extends ConsumerWidget {
  final String dataKey;
  
  const TemporaryDataWidget({required this.dataKey, Key? key}) : super(key: key);
  
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final data = ref.watch(temporaryDataProvider(dataKey));
    
    return Column(
      children: [
        Text('Data: $data'),
        ElevatedButton(
          onPressed: () {
            ref.read(temporaryDataProvider(dataKey).notifier).state = 'Updated data';
          },
          child: Text('Update'),
        ),
      ],
    );
  }
}
```

**Performance Benefits:**

**১. Caching:**
```dart
// প্রতিটি userId-এর জন্য আলাদা cache
final userProvider = FutureProvider.family<User, String>((ref, userId) async {
  // Same userId-এর জন্য same data return করবে
  final response = await http.get(Uri.parse('https://api.example.com/users/$userId'));
  return User.fromJson(jsonDecode(response.body));
});
```

**২. Selective Rebuild:**
```dart
class UserListWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    return ListView.builder(
      itemCount: userIds.length,
      itemBuilder: (context, index) {
        final userId = userIds[index];
        // শুধুমাত্র এই specific user-এর data change হলে rebuild হবে
        final userAsync = ref.watch(userProvider(userId));
        
        return userAsync.when(
          data: (user) => ListTile(title: Text(user.name)),
          loading: () => ListTile(title: Text('Loading...')),
          error: (error, stack) => ListTile(title: Text('Error')),
        );
      },
    );
  }
}
```

**Pitfalls:**
1. `family` parameter-এ complex object রাখলে performance issue হতে পারে
2. `family` + `autoDispose` ব্যবহার করলে parameter change হলে state reset হয়
3. `family` parameter-এ mutable object ব্যবহার করলে unexpected behavior হতে পারে

**Interview Tips:** 
- Parameterized data → `family`
- Per-instance state → `family`
- Cached data by parameter → `family`
- `family` + `autoDispose` = per-parameter scoped lifecycle


