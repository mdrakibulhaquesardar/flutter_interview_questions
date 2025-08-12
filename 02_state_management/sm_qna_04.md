### প্রশ্ন ১৬: MVVM/MVP/BLoC—Flutter-এ কোন আর্কিটেকচার ভালো?

**উত্তর (ডিটেইল):**

**Flutter-এ আর্কিটেকচারাল প্যাটার্নের গুরুত্ব:**

Flutter-এ আর্কিটেকচারাল প্যাটার্ন ব্যবহার করা খুবই গুরুত্বপূর্ণ কারণ এটি কোডকে স্ট্রাকচার্ড, টেস্টেবল, এবং মেইনটেইনেবল করে তোলে। Flutter-এর declarative nature-এর কারণে কিছু প্যাটার্ন অন্যদের চেয়ে বেশি উপযুক্ত।

**MVVM (Model-View-ViewModel):**

MVVM Flutter-এ খুবই জনপ্রিয় কারণ এটি Flutter-এর declarative UI-এর সাথে খুব ভালোভাবে কাজ করে।

**MVVM-এর মূল কম্পোনেন্টগুলো:**

1. **Model**: ডেটা এবং বিজনেস লজিক। এটি UI থেকে সম্পূর্ণ আলাদা থাকে।
2. **View**: UI কম্পোনেন্ট (Widget)। এটি শুধুমাত্র UI রেন্ডার করে এবং user interaction হ্যান্ডেল করে।
3. **ViewModel**: View এবং Model-এর মধ্যে bridge। এটি UI state ম্যানেজ করে এবং Model-এর সাথে যোগাযোগ করে।

**MVVM-এর সুবিধাগুলো:**

- **Separation of Concerns**: UI, বিজনেস লজিক, এবং ডেটা আলাদা থাকে
- **Testability**: ViewModel আলাদাভাবে টেস্ট করা যায়
- **Reusability**: ViewModel বিভিন্ন View-এ ব্যবহার করা যায়
- **Flutter Compatibility**: Flutter-এর reactive nature-এর সাথে খুব ভালোভাবে কাজ করে

**উদাহরণ:**

```dart
// Model
class User {
  final String name;
  final String email;
  
  User({required this.name, required this.email});
}

// ViewModel
class UserViewModel extends ChangeNotifier {
  User? _user;
  bool _isLoading = false;
  String? _error;
  
  User? get user => _user;
  bool get isLoading => _isLoading;
  String? get error => _error;
  
  Future<void> loadUser(String userId) async {
    _isLoading = true;
    _error = null;
    notifyListeners();
    
    try {
      final user = await UserRepository().getUser(userId);
      _user = user;
    } catch (e) {
      _error = e.toString();
    } finally {
      _isLoading = false;
      notifyListeners();
    }
  }
}

// View
class UserProfilePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (_) => UserViewModel(),
      child: Consumer<UserViewModel>(
        builder: (context, viewModel, child) {
          if (viewModel.isLoading) {
            return CircularProgressIndicator();
          }
          
          if (viewModel.error != null) {
            return Text('Error: ${viewModel.error}');
          }
          
          final user = viewModel.user;
          if (user == null) {
            return Text('No user data');
          }
          
          return Column(
            children: [
              Text('Name: ${user.name}'),
              Text('Email: ${user.email}'),
            ],
          );
        },
      ),
    );
  }
}
```

**BLoC (Business Logic Component):**

BLoC Flutter-এর জন্য বিশেষভাবে ডিজাইন করা একটি আর্কিটেকচারাল প্যাটার্ন।

**BLoC-এর মূল কম্পোনেন্টগুলো:**

1. **Events**: User interaction বা system events যা BLoC-এ পাঠানো হয়
2. **States**: UI-এর বর্তমান state যা BLoC থেকে আসে
3. **BLoC**: Events গ্রহণ করে এবং States তৈরি করে

**BLoC-এর সুবিধাগুলো:**

- **Predictable State Changes**: Event-driven architecture
- **Testability**: Events এবং States আলাদাভাবে টেস্ট করা যায়
- **Reusability**: BLoC বিভিন্ন UI-এ ব্যবহার করা যায়
- **Separation of Concerns**: UI এবং বিজনেস লজিক সম্পূর্ণ আলাদা

**উদাহরণ:**

```dart
// Events
abstract class UserEvent {}

class LoadUser extends UserEvent {
  final String userId;
  LoadUser(this.userId);
}

// States
abstract class UserState {}

class UserInitial extends UserState {}
class UserLoading extends UserState {}
class UserLoaded extends UserState {
  final User user;
  UserLoaded(this.user);
}
class UserError extends UserState {
  final String message;
  UserError(this.message);
}

// BLoC
class UserBloc extends Bloc<UserEvent, UserState> {
  UserBloc() : super(UserInitial()) {
    on<LoadUser>(_onLoadUser);
  }
  
  Future<void> _onLoadUser(LoadUser event, Emitter<UserState> emit) async {
    emit(UserLoading());
    
    try {
      final user = await UserRepository().getUser(event.userId);
      emit(UserLoaded(user));
    } catch (e) {
      emit(UserError(e.toString()));
    }
  }
}

// View
class UserProfilePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (context) => UserBloc(),
      child: BlocBuilder<UserBloc, UserState>(
        builder: (context, state) {
          if (state is UserLoading) {
            return CircularProgressIndicator();
          }
          
          if (state is UserError) {
            return Text('Error: ${state.message}');
          }
          
          if (state is UserLoaded) {
            return Column(
              children: [
                Text('Name: ${state.user.name}'),
                Text('Email: ${state.user.email}'),
              ],
            );
          }
          
          return Text('No user data');
        },
      ),
    );
  }
}
```

**MVP (Model-View-Presenter):**

MVP Flutter-এ কম ব্যবহৃত হয় কারণ Flutter-এর declarative nature-এর সাথে এটি খুব ভালোভাবে কাজ করে না।

**MVP-এর সমস্যাগুলো:**

- **Over-abstraction**: Presenter অতিরিক্ত abstraction তৈরি করতে পারে
- **Flutter Incompatibility**: Flutter-এর reactive nature-এর সাথে conflict
- **Complexity**: ছোট প্রজেক্টের জন্য অতিরিক্ত complexity

**কখন কোন আর্কিটেকচার ব্যবহার করবেন:**

1. **MVVM**: 
   - ছোট থেকে মাঝারি প্রজেক্ট
   - টিম Flutter-এ নতুন
   - দ্রুত প্রোটোটাইপিং প্রয়োজন

2. **BLoC**: 
   - বড় এবং জটিল প্রজেক্ট
   - টিম BLoC-এ অভিজ্ঞ
   - Predictable state management প্রয়োজন

3. **MVP**: 
   - Flutter-এ সাধারণত ব্যবহার করা হয় না
   - Legacy code migration-এ ব্যবহার করা যেতে পারে

**Pitfalls:**
1. একটি আর্কিটেকচার বেছে নেওয়ার আগে টিমের দক্ষতা বিবেচনা করুন
2. প্রজেক্টের complexity অনুযায়ী আর্কিটেকচার বেছে নিন
3. Over-engineering এড়িয়ে চলুন

**Interview Tips:** 
- MVVM এবং BLoC Flutter-এ সবচেয়ে জনপ্রিয়
- BLoC শেখার কার্ভ বেশি কিন্তু বেশি powerful
- টিমের দক্ষতা এবং প্রজেক্টের complexity বিবেচনায় আর্কিটেকচার বেছে নিন
- একটি আর্কিটেকচার বেছে নেওয়ার পর সেটি পুরো প্রজেক্টে consistently ব্যবহার করুন

---

### প্রশ্ন ১৭: Repository প্যাটার্নের ভূমিকা কী?

**উত্তর (ডিটেইল):**

**Repository Pattern কী:**

Repository Pattern হলো একটি ডিজাইন প্যাটার্ন যা ডেটা অ্যাক্সেস লজিককে অ্যাপ্লিকেশনের বাকি অংশ থেকে আলাদা করে। এটি একটি abstraction layer তৈরি করে যা বিভিন্ন ডেটা সোর্স (API, Database, Cache) কে একত্রিত করে একটি unified interface প্রদান করে।

**Repository Pattern-এর মূল উদ্দেশ্য:**

1. **Data Source Abstraction**: বিভিন্ন ডেটা সোর্স (API, Local Database, Cache) কে একটি interface-এর মাধ্যমে access করা
2. **Separation of Concerns**: UI এবং বিজনেস লজিক থেকে ডেটা অ্যাক্সেস লজিক আলাদা করা
3. **Testability**: Repository-কে mock করে সহজে টেস্ট করা
4. **Flexibility**: ডেটা সোর্স পরিবর্তন করা সহজ (API থেকে Local DB-তে migration)

**Repository Pattern-এর কম্পোনেন্টগুলো:**

1. **Repository Interface**: ডেটা অ্যাক্সেসের জন্য contract define করে
2. **Repository Implementation**: বিভিন্ন ডেটা সোর্সের সাথে কাজ করে
3. **Data Models**: ডেটা স্ট্রাকচার define করে
4. **Data Sources**: API, Database, Cache ইত্যাদি

**উদাহরণ:**

```dart
// 1. Data Model
class User {
  final String id;
  final String name;
  final String email;
  
  User({
    required this.id,
    required this.name,
    required this.email,
  });
  
  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'],
      name: json['name'],
      email: json['email'],
    );
  }
  
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
      'email': email,
    };
  }
}

// 2. Repository Interface
abstract class UserRepository {
  Future<List<User>> getUsers();
  Future<User?> getUser(String id);
  Future<void> saveUser(User user);
  Future<void> deleteUser(String id);
}

// 3. API Implementation
class ApiUserRepository implements UserRepository {
  final http.Client _client;
  final String _baseUrl;
  
  ApiUserRepository({
    http.Client? client,
    String baseUrl = 'https://api.example.com',
  }) : _client = client ?? http.Client(),
       _baseUrl = baseUrl;
  
  @override
  Future<List<User>> getUsers() async {
    try {
      final response = await _client.get(Uri.parse('$_baseUrl/users'));
      
      if (response.statusCode == 200) {
        final List<dynamic> jsonList = json.decode(response.body);
        return jsonList.map((json) => User.fromJson(json)).toList();
      } else {
        throw Exception('Failed to load users: ${response.statusCode}');
      }
    } catch (e) {
      throw Exception('Network error: $e');
    }
  }
  
  @override
  Future<User?> getUser(String id) async {
    try {
      final response = await _client.get(Uri.parse('$_baseUrl/users/$id'));
      
      if (response.statusCode == 200) {
        final json = json.decode(response.body);
        return User.fromJson(json);
      } else if (response.statusCode == 404) {
        return null;
      } else {
        throw Exception('Failed to load user: ${response.statusCode}');
      }
    } catch (e) {
      throw Exception('Network error: $e');
    }
  }
  
  @override
  Future<void> saveUser(User user) async {
    try {
      final response = await _client.post(
        Uri.parse('$_baseUrl/users'),
        headers: {'Content-Type': 'application/json'},
        body: json.encode(user.toJson()),
      );
      
      if (response.statusCode != 201) {
        throw Exception('Failed to save user: ${response.statusCode}');
      }
    } catch (e) {
      throw Exception('Network error: $e');
    }
  }
  
  @override
  Future<void> deleteUser(String id) async {
    try {
      final response = await _client.delete(Uri.parse('$_baseUrl/users/$id'));
      
      if (response.statusCode != 204) {
        throw Exception('Failed to delete user: ${response.statusCode}');
      }
    } catch (e) {
      throw Exception('Network error: $e');
    }
  }
}

// 4. Local Database Implementation
class LocalUserRepository implements UserRepository {
  final Database _database;
  
  LocalUserRepository(this._database);
  
  @override
  Future<List<User>> getUsers() async {
    final List<Map<String, dynamic>> maps = await _database.query('users');
    return List.generate(maps.length, (i) => User.fromJson(maps[i]));
  }
  
  @override
  Future<User?> getUser(String id) async {
    final List<Map<String, dynamic>> maps = await _database.query(
      'users',
      where: 'id = ?',
      whereArgs: [id],
    );
    
    if (maps.isNotEmpty) {
      return User.fromJson(maps.first);
    }
    return null;
  }
  
  @override
  Future<void> saveUser(User user) async {
    await _database.insert(
      'users',
      user.toJson(),
      conflictAlgorithm: ConflictAlgorithm.replace,
    );
  }
  
  @override
  Future<void> deleteUser(String id) async {
    await _database.delete(
      'users',
      where: 'id = ?',
      whereArgs: [id],
    );
  }
}

// 5. Cached Repository Implementation
class CachedUserRepository implements UserRepository {
  final UserRepository _remoteRepository;
  final UserRepository _localRepository;
  final Duration _cacheExpiration;
  
  CachedUserRepository({
    required UserRepository remoteRepository,
    required UserRepository localRepository,
    Duration cacheExpiration = const Duration(minutes: 5),
  }) : _remoteRepository = remoteRepository,
       _localRepository = localRepository,
       _cacheExpiration = cacheExpiration;
  
  @override
  Future<List<User>> getUsers() async {
    try {
      // First try to get from cache
      final cachedUsers = await _localRepository.getUsers();
      if (cachedUsers.isNotEmpty) {
        return cachedUsers;
      }
      
      // If cache is empty, get from remote
      final remoteUsers = await _remoteRepository.getUsers();
      
      // Save to cache
      for (final user in remoteUsers) {
        await _localRepository.saveUser(user);
      }
      
      return remoteUsers;
    } catch (e) {
      // If remote fails, try cache
      final cachedUsers = await _localRepository.getUsers();
      if (cachedUsers.isNotEmpty) {
        return cachedUsers;
      }
      throw e;
    }
  }
  
  @override
  Future<User?> getUser(String id) async {
    try {
      // First try cache
      final cachedUser = await _localRepository.getUser(id);
      if (cachedUser != null) {
        return cachedUser;
      }
      
      // If not in cache, get from remote
      final remoteUser = await _remoteRepository.getUser(id);
      
      // Save to cache if found
      if (remoteUser != null) {
        await _localRepository.saveUser(remoteUser);
      }
      
      return remoteUser;
    } catch (e) {
      // If remote fails, try cache
      return await _localRepository.getUser(id);
    }
  }
  
  @override
  Future<void> saveUser(User user) async {
    // Save to both remote and local
    await Future.wait([
      _remoteRepository.saveUser(user),
      _localRepository.saveUser(user),
    ]);
  }
  
  @override
  Future<void> deleteUser(String id) async {
    // Delete from both remote and local
    await Future.wait([
      _remoteRepository.deleteUser(id),
      _localRepository.deleteUser(id),
    ]);
  }
}

// 6. Usage in ViewModel/BLoC
class UserViewModel extends ChangeNotifier {
  final UserRepository _repository;
  
  UserViewModel(this._repository);
  
  List<User> _users = [];
  bool _isLoading = false;
  String? _error;
  
  List<User> get users => _users;
  bool get isLoading => _isLoading;
  String? get error => _error;
  
  Future<void> loadUsers() async {
    _isLoading = true;
    _error = null;
    notifyListeners();
    
    try {
      _users = await _repository.getUsers();
    } catch (e) {
      _error = e.toString();
    } finally {
      _isLoading = false;
      notifyListeners();
    }
  }
}
```

**Repository Pattern-এর সুবিধাগুলো:**

1. **Separation of Concerns**: ডেটা অ্যাক্সেস লজিক UI থেকে আলাদা থাকে
2. **Testability**: Repository-কে mock করে সহজে টেস্ট করা যায়
3. **Flexibility**: ডেটা সোর্স পরিবর্তন করা সহজ
4. **Caching Strategy**: বিভিন্ন caching strategy implement করা যায়
5. **Error Handling**: Centralized error handling
6. **Offline Support**: Offline-first architecture implement করা যায়

**Repository Pattern-এর Best Practices:**

1. **Interface-based Design**: Always use interfaces for repositories
2. **Single Responsibility**: Each repository should handle one type of data
3. **Error Handling**: Proper error handling and propagation
4. **Caching Strategy**: Implement appropriate caching strategies
5. **Testing**: Mock repositories for testing
6. **Dependency Injection**: Use DI to inject repositories

**Pitfalls:**
1. Over-engineering for simple apps
2. Not handling errors properly
3. Not implementing proper caching strategies
4. Mixing business logic with data access logic

**Interview Tips:** 
- Repository pattern Flutter-এ খুবই গুরুত্বপূর্ণ
- Clean Architecture-এর সাথে খুব ভালোভাবে কাজ করে
- Offline-first apps-এর জন্য essential
- Testing-এ খুবই উপকারী
- Caching strategy implement করার সময় memory management খেয়াল রাখুন

---

### প্রশ্ন ১৮: State immutability কেন গুরুত্বপূর্ণ?

**উত্তর (ডিটেইল):**

- ইম্যুটেবল স্টেট ডিফ/ইকুয়ালিটি সহজ করে; অনাকাঙ্ক্ষিত সাইড-ইফেক্ট কমে।
- Equatable/freezed দিয়ে ভ্যালু ইকুয়ালিটি নিশ্চিত করুন।

**Interview Tips:** কপি-উইথ প্যাটার্ন (`copyWith`) স্ট্যান্ডার্ড।

---

### প্রশ্ন ১৯: Form state ম্যানেজমেন্ট কিভাবে করবেন?

**উত্তর (ডিটেইল):**

- সিম্পল ফর্ম → `Form` + `TextEditingController` + লোকাল `setState`।
- কমপ্লেক্স → Provider/Riverpod/BLoC-এ ফিল্ড স্টেট/ভ্যালিডেশন/সাবমিট স্টেট ম্যানেজ।

**Interview Tips:** ভ্যালিডেশন UI-লজিক আলাদা রাখুন; সাবমিট স্টেট (loading) কেন্দ্রীভূত করুন।

---

### প্রশ্ন ২০: Error/Loading স্টেট ডিজাইন বেস্ট প্র্যাকটিস?

**উত্তর (ডিটেইল):**

- Explicit state model: `Loading`, `Success<T>`, `Error(e)` সিল্ড/ইউনিয়ন টাইপ।
- UI-তে তিন স্টেট পরিষ্কার ম্যাপ করুন; রিট্রাই অ্যাকশন দিন।

**Interview Tips:** স্টেট টাইপ-সেফ করতে `sealed classes`/`freezed` চমৎকার।


