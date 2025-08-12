### প্রশ্ন ২১: Provider-এ multi-provider সেটআপ কিভাবে করবেন?

**উত্তর (ডিটেইল):**

**MultiProvider কী:**

MultiProvider হলো Provider প্যাকেজের একটি widget যা একসাথে একাধিক provider সেটআপ করতে সাহায্য করে। এটি dependency injection-এর জন্য খুবই উপকারী।

**MultiProvider-এর সুবিধাগুলো:**

1. **Centralized Setup**: সব provider এক জায়গায় সেটআপ করা যায়
2. **Dependency Management**: Provider-গুলোর মধ্যে dependency manage করা যায়
3. **Clean Code**: কোড organized এবং readable থাকে
4. **Testing**: Provider-গুলো সহজে mock করা যায়

**MultiProvider-এর বেসিক সেটআপ:**

```dart
void main() {
  runApp(
    MultiProvider(
      providers: [
        ChangeNotifierProvider(create: (_) => AuthProvider()),
        ChangeNotifierProvider(create: (_) => UserProvider()),
        ChangeNotifierProvider(create: (_) => ThemeProvider()),
      ],
      child: MyApp(),
    ),
  );
}
```

**Provider Order এবং Dependency:**

Provider-গুলোর order গুরুত্বপূর্ণ কারণ dependency-গুলো আগে define করতে হয়:

```dart
MultiProvider(
  providers: [
    // 1. Base services (API client, database)
    Provider<ApiClient>(
      create: (_) => ApiClient(),
    ),
    
    // 2. Repositories (depend on services)
    ProxyProvider<ApiClient, UserRepository>(
      create: (context) => UserRepository(
        apiClient: context.read<ApiClient>(),
      ),
    ),
    
    // 3. ViewModels/Notifiers (depend on repositories)
    ChangeNotifierProxyProvider<UserRepository, UserNotifier>(
      create: (context) => UserNotifier(
        repository: context.read<UserRepository>(),
      ),
      update: (context, repository, previous) => 
        previous ?? UserNotifier(repository: repository),
    ),
    
    // 4. UI-specific providers
    ChangeNotifierProvider(create: (_) => ThemeProvider()),
    ChangeNotifierProvider(create: (_) => LanguageProvider()),
  ],
  child: MyApp(),
)
```

**Feature-scoped Providers:**

বড় অ্যাপে feature-specific provider-গুলো আলাদা widget-এ রাখা ভালো:

```dart
// User feature providers
class UserFeatureProviders extends StatelessWidget {
  final Widget child;
  
  const UserFeatureProviders({required this.child, super.key});
  
  @override
  Widget build(BuildContext context) {
    return MultiProvider(
      providers: [
        ChangeNotifierProvider(create: (_) => UserListNotifier()),
        ChangeNotifierProvider(create: (_) => UserDetailNotifier()),
        ChangeNotifierProvider(create: (_) => UserSearchNotifier()),
      ],
      child: child,
    );
  }
}

// Usage
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MultiProvider(
      providers: [
        // Global providers
        ChangeNotifierProvider(create: (_) => AuthProvider()),
        ChangeNotifierProvider(create: (_) => ThemeProvider()),
      ],
      child: MaterialApp(
        home: UserFeatureProviders(
          child: UserListPage(),
        ),
      ),
    );
  }
}
```

**Provider-এর Best Practices:**

1. **Keep providers close to where they're used**: Global providers শুধুমাত্র global state-এর জন্য
2. **Use ProxyProvider for dependencies**: Avoid manual dependency injection
3. **Lazy loading**: Provider-গুলো lazy load করুন
4. **Proper disposal**: Provider-গুলো properly dispose করুন
5. **Testing**: Provider-গুলো mock করে test করুন

**Pitfalls:**
1. Provider order ভুল হলে dependency error হবে
2. Too many global providers performance impact করবে
3. Not disposing providers properly memory leak করবে
4. Circular dependency avoid করতে হবে

**Interview Tips:** 
- MultiProvider dependency injection-এর জন্য standard approach
- Provider order গুরুত্বপূর্ণ - dependencies আগে define করতে হবে
- Feature-scoped providers performance-এর জন্য ভালো
- ProxyProvider complex dependencies-এর জন্য ব্যবহার করুন
- Testing-এ provider mock করা সহজ

---

### প্রশ্ন ২২: Provider-এ `ProxyProvider`/`ChangeNotifierProxyProvider` কবে দরকার?

**উত্তর (ডিটেইল):**

**ProxyProvider কী:**

ProxyProvider হলো Provider প্যাকেজের একটি বিশেষ provider যা অন্য provider-এর value থেকে নতুন provider তৈরি করে। এটি dependency injection-এর জন্য খুবই গুরুত্বপূর্ণ।

**ProxyProvider-এর ব্যবহার:**

1. **Dependency Injection**: অন্য provider-এর value inject করা
2. **Computed Values**: অন্য provider-এর value থেকে computed value তৈরি করা
3. **Service Configuration**: Configuration-এর উপর ভিত্তি করে service তৈরি করা
4. **State Synchronization**: Multiple provider-এর state synchronize করা

**ProxyProvider-এর বিভিন্ন ধরন:**

**1. ProxyProvider (Simple):**

```dart
MultiProvider(
  providers: [
    // Base provider
    ChangeNotifierProvider(create: (_) => AuthProvider()),
    
    // Proxy provider that depends on AuthProvider
    ProxyProvider<AuthProvider, UserService>(
      create: (context) => UserService(
        authToken: context.read<AuthProvider>().token,
      ),
      update: (context, authProvider, previous) => 
        previous ?? UserService(authToken: authProvider.token),
    ),
  ],
  child: MyApp(),
)
```

**2. ChangeNotifierProxyProvider:**

```dart
MultiProvider(
  providers: [
    // Base provider
    ChangeNotifierProvider(create: (_) => AuthProvider()),
    
    // ChangeNotifier proxy provider
    ChangeNotifierProxyProvider<AuthProvider, UserNotifier>(
      create: (context) => UserNotifier(
        authProvider: context.read<AuthProvider>(),
      ),
      update: (context, authProvider, previous) => 
        previous ?? UserNotifier(authProvider: authProvider),
    ),
  ],
  child: MyApp(),
)
```

**3. ProxyProvider2 (Multiple Dependencies):**

```dart
MultiProvider(
  providers: [
    // Multiple base providers
    ChangeNotifierProvider(create: (_) => AuthProvider()),
    ChangeNotifierProvider(create: (_) => ThemeProvider()),
    
    // Proxy provider with multiple dependencies
    ProxyProvider2<AuthProvider, ThemeProvider, AppConfig>(
      create: (context) => AppConfig(
        authProvider: context.read<AuthProvider>(),
        themeProvider: context.read<ThemeProvider>(),
      ),
      update: (context, authProvider, themeProvider, previous) => 
        previous ?? AppConfig(
          authProvider: authProvider,
          themeProvider: themeProvider,
        ),
    ),
  ],
  child: MyApp(),
)
```

**4. ProxyProvider3 (Three Dependencies):**

```dart
MultiProvider(
  providers: [
    ChangeNotifierProvider(create: (_) => AuthProvider()),
    ChangeNotifierProvider(create: (_) => ThemeProvider()),
    ChangeNotifierProvider(create: (_) => LanguageProvider()),
    
    ProxyProvider3<AuthProvider, ThemeProvider, LanguageProvider, AppService>(
      create: (context) => AppService(
        authProvider: context.read<AuthProvider>(),
        themeProvider: context.read<ThemeProvider>(),
        languageProvider: context.read<LanguageProvider>(),
      ),
      update: (context, authProvider, themeProvider, languageProvider, previous) => 
        previous ?? AppService(
          authProvider: authProvider,
          themeProvider: themeProvider,
          languageProvider: languageProvider,
        ),
    ),
  ],
  child: MyApp(),
)
```

**ProxyProvider-এর ব্যবহারের উদাহরণ:**

**1. API Client Configuration:**

```dart
MultiProvider(
  providers: [
    // Auth provider
    ChangeNotifierProvider(create: (_) => AuthProvider()),
    
    // API client that depends on auth
    ProxyProvider<AuthProvider, ApiClient>(
      create: (context) => ApiClient(
        baseUrl: 'https://api.example.com',
        authToken: context.read<AuthProvider>().token,
      ),
      update: (context, authProvider, previous) => 
        previous ?? ApiClient(
          baseUrl: 'https://api.example.com',
          authToken: authProvider.token,
        ),
    ),
    
    // Repository that depends on API client
    ProxyProvider<ApiClient, UserRepository>(
      create: (context) => UserRepository(
        apiClient: context.read<ApiClient>(),
      ),
      update: (context, apiClient, previous) => 
        previous ?? UserRepository(apiClient: apiClient),
    ),
  ],
  child: MyApp(),
)
```

**2. Theme-based Configuration:**

```dart
MultiProvider(
  providers: [
    ChangeNotifierProvider(create: (_) => ThemeProvider()),
    
    ProxyProvider<ThemeProvider, AppConfig>(
      create: (context) => AppConfig(
        isDarkMode: context.read<ThemeProvider>().isDarkMode,
        primaryColor: context.read<ThemeProvider>().primaryColor,
      ),
      update: (context, themeProvider, previous) => 
        previous ?? AppConfig(
          isDarkMode: themeProvider.isDarkMode,
          primaryColor: themeProvider.primaryColor,
        ),
    ),
  ],
  child: MyApp(),
)
```

**3. Session Management:**

```dart
MultiProvider(
  providers: [
    ChangeNotifierProvider(create: (_) => AuthProvider()),
    
    ProxyProvider<AuthProvider, SessionManager>(
      create: (context) => SessionManager(
        isLoggedIn: context.read<AuthProvider>().isLoggedIn,
        userId: context.read<AuthProvider>().userId,
      ),
      update: (context, authProvider, previous) => 
        previous ?? SessionManager(
          isLoggedIn: authProvider.isLoggedIn,
          userId: authProvider.userId,
        ),
    ),
  ],
  child: MyApp(),
)
```

**ProxyProvider-এর Best Practices:**

1. **Use update callback**: Always provide update callback for proper updates
2. **Handle previous value**: Check previous value to avoid unnecessary recreations
3. **Keep dependencies minimal**: Don't depend on too many providers
4. **Use appropriate provider type**: Choose the right proxy provider type
5. **Test thoroughly**: Test proxy provider updates

**ProxyProvider Testing:**

```dart
testWidgets('ProxyProvider test', (WidgetTester tester) async {
  final mockAuthProvider = MockAuthProvider();
  
  await tester.pumpWidget(
    MultiProvider(
      providers: [
        ChangeNotifierProvider.value(value: mockAuthProvider),
        ProxyProvider<AuthProvider, UserService>(
          create: (context) => UserService(
            authToken: context.read<AuthProvider>().token,
          ),
          update: (context, authProvider, previous) => 
            previous ?? UserService(authToken: authProvider.token),
        ),
      ],
      child: MyWidget(),
    ),
  );
  
  // Test your widget
  expect(find.text('User Service'), findsOneWidget);
});
```

**Pitfalls:**
1. Not providing update callback
2. Circular dependencies
3. Too many dependencies in one proxy provider
4. Not handling previous value properly

**Interview Tips:** 
- ProxyProvider dependency injection-এর জন্য essential
- update callback always provide করতে হবে
- Circular dependency avoid করতে হবে
- Testing-এ proxy provider properly test করতে হবে
- Performance-এর জন্য minimal dependencies রাখতে হবে

---

### প্রশ্ন ২৩: Riverpod-এ dependency override/testing কিভাবে?

**উত্তর (ডিটেইল):**

**Riverpod Dependency Override:**

Riverpod-এ dependency override করা খুবই সহজ এবং powerful। এটি testing এবং development-এ খুবই উপকারী।

**Provider Override-এর বিভিন্ন ধরন:**

**1. overrideWithValue:**

```dart
// Original provider
final userProvider = StateNotifierProvider<UserNotifier, UserState>((ref) {
  return UserNotifier();
});

// Override with mock value
final container = ProviderContainer(
  overrides: [
    userProvider.overrideWithValue(UserState(name: 'Mock User')),
  ],
);

// Usage
final user = container.read(userProvider);
```

**2. overrideWithProvider:**

```dart
// Mock provider
final mockUserProvider = StateNotifierProvider<UserNotifier, UserState>((ref) {
  return MockUserNotifier();
});

// Override with another provider
final container = ProviderContainer(
  overrides: [
    userProvider.overrideWithProvider(mockUserProvider),
  ],
);
```

**3. overrideWithProvider (Different Type):**

```dart
// Override with different provider type
final container = ProviderContainer(
  overrides: [
    userProvider.overrideWithProvider(
      FutureProvider<UserState>((ref) async {
        return UserState(name: 'Async Mock User');
      }),
    ),
  ],
);
```

**Testing-এ Provider Override:**

**1. Widget Testing:**

```dart
testWidgets('User widget test', (WidgetTester tester) async {
  await tester.pumpWidget(
    ProviderScope(
      overrides: [
        userProvider.overrideWithValue(
          UserState(name: 'Test User', age: 25),
        ),
      ],
      child: UserWidget(),
    ),
  );
  
  expect(find.text('Test User'), findsOneWidget);
  expect(find.text('25'), findsOneWidget);
});
```

**2. Unit Testing:**

```dart
test('User notifier test', () {
  final container = ProviderContainer(
    overrides: [
      userRepositoryProvider.overrideWithProvider(
        Provider<UserRepository>((ref) => MockUserRepository()),
      ),
    ],
  );
  
  final userNotifier = container.read(userProvider.notifier);
  final userState = container.read(userProvider);
  
  expect(userState.name, 'Mock User');
});
```

**3. Integration Testing:**

```dart
testWidgets('User flow test', (WidgetTester tester) async {
  await tester.pumpWidget(
    ProviderScope(
      overrides: [
        userProvider.overrideWithProvider(
          StateNotifierProvider<UserNotifier, UserState>((ref) {
            return TestUserNotifier();
          }),
        ),
      ],
      child: MyApp(),
    ),
  );
  
  // Test user flow
  await tester.tap(find.byType(ElevatedButton));
  await tester.pump();
  
  expect(find.text('Updated User'), findsOneWidget);
});
```

**Provider Override-এর Advanced Features:**

**1. Conditional Override:**

```dart
final container = ProviderContainer(
  overrides: [
    if (kDebugMode)
      userProvider.overrideWithValue(
        UserState(name: 'Debug User'),
      ),
  ],
);
```

**2. Multiple Overrides:**

```dart
final container = ProviderContainer(
  overrides: [
    userProvider.overrideWithValue(UserState(name: 'Mock User')),
    themeProvider.overrideWithValue(ThemeData.dark()),
    languageProvider.overrideWithValue('en'),
  ],
);
```

**3. Nested Override:**

```dart
final container = ProviderContainer(
  overrides: [
    userProvider.overrideWithProvider(
      StateNotifierProvider<UserNotifier, UserState>((ref) {
        // This provider can also be overridden
        return UserNotifier(
          repository: ref.read(userRepositoryProvider),
        );
      }),
    ),
  ],
);
```

**Provider Override-এর Best Practices:**

**1. Use for Testing:**

```dart
// Test file
void main() {
  group('User Tests', () {
    late ProviderContainer container;
    
    setUp(() {
      container = ProviderContainer(
        overrides: [
          userRepositoryProvider.overrideWithProvider(
            Provider<UserRepository>((ref) => MockUserRepository()),
          ),
        ],
      );
    });
    
    tearDown(() {
      container.dispose();
    });
    
    test('should load user', () async {
      final userNotifier = container.read(userProvider.notifier);
      await userNotifier.loadUser('123');
      
      final user = container.read(userProvider);
      expect(user.name, 'Mock User');
    });
  });
}
```

**2. Use for Development:**

```dart
// Development configuration
final container = ProviderContainer(
  overrides: [
    if (kDebugMode) ...[
      apiClientProvider.overrideWithProvider(
        Provider<ApiClient>((ref) => MockApiClient()),
      ),
      analyticsProvider.overrideWithProvider(
        Provider<Analytics>((ref) => NoOpAnalytics()),
      ),
    ],
  ],
);
```

**3. Use for Feature Flags:**

```dart
final container = ProviderContainer(
  overrides: [
    if (FeatureFlags.useNewUI)
      uiProvider.overrideWithProvider(
        Provider<UITheme>((ref) => NewUITheme()),
      ),
  ],
);
```

**Provider Override-এর Performance Considerations:**

**1. Lazy Override:**

```dart
final container = ProviderContainer(
  overrides: [
    expensiveProvider.overrideWithProvider(
      Provider<ExpensiveService>((ref) {
        // Only create when needed
        return ExpensiveService();
      }),
    ),
  ],
);
```

**2. Cached Override:**

```dart
final mockUserRepository = MockUserRepository();

final container = ProviderContainer(
  overrides: [
    userRepositoryProvider.overrideWithValue(mockUserRepository),
  ],
);
```

**Provider Override-এর Pitfalls:**

1. **Memory Leaks**: Not disposing containers properly
2. **Circular Dependencies**: Creating circular override dependencies
3. **Performance Issues**: Overriding expensive providers unnecessarily
4. **Testing Issues**: Not properly mocking async operations

**Provider Override-এর Interview Tips:**

- Provider override Riverpod-এর সবচেয়ে powerful feature
- Testing-এ খুবই উপকারী
- Development-এ mock data ব্যবহার করা যায়
- Feature flags implement করা যায়
- Performance-এর জন্য careful হতে হবে

---

### প্রশ্ন ২৪: BLoC-এ side-effect (API কল) কোথায় রাখবেন?

**উত্তর (ডিটেইল):**

**BLoC-এ Side Effects:**

BLoC-এ side effects (API calls, database operations, etc.) event handler-এ রাখা হয়। এটি BLoC pattern-এর একটি গুরুত্বপূর্ণ অংশ।

**Side Effects-এর ধরন:**

1. **API Calls**: Network requests
2. **Database Operations**: Local data operations
3. **File Operations**: File read/write
4. **Analytics**: User behavior tracking
5. **Notifications**: Push notifications

**BLoC-এ Side Effects-এর Best Practices:**

**1. Event Handler-এ Side Effects:**

```dart
class UserBloc extends Bloc<UserEvent, UserState> {
  final UserRepository _repository;
  
  UserBloc(this._repository) : super(UserInitial()) {
    on<LoadUser>(_onLoadUser);
    on<UpdateUser>(_onUpdateUser);
    on<DeleteUser>(_onDeleteUser);
  }
  
  // API call in event handler
  Future<void> _onLoadUser(LoadUser event, Emitter<UserState> emit) async {
    emit(UserLoading());
    
    try {
      final user = await _repository.getUser(event.userId);
      emit(UserLoaded(user));
    } catch (e) {
      emit(UserError(e.toString()));
    }
  }
  
  // Database operation in event handler
  Future<void> _onUpdateUser(UpdateUser event, Emitter<UserState> emit) async {
    emit(UserUpdating());
    
    try {
      final updatedUser = await _repository.updateUser(event.user);
      emit(UserUpdated(updatedUser));
    } catch (e) {
      emit(UserError(e.toString()));
    }
  }
  
  // File operation in event handler
  Future<void> _onDeleteUser(DeleteUser event, Emitter<UserState> emit) async {
    emit(UserDeleting());
    
    try {
      await _repository.deleteUser(event.userId);
      emit(UserDeleted());
    } catch (e) {
      emit(UserError(e.toString()));
    }
  }
}
```

**2. Repository Pattern ব্যবহার:**

```dart
// Repository interface
abstract class UserRepository {
  Future<User> getUser(String id);
  Future<User> updateUser(User user);
  Future<void> deleteUser(String id);
}

// API implementation
class ApiUserRepository implements UserRepository {
  final ApiClient _apiClient;
  
  ApiUserRepository(this._apiClient);
  
  @override
  Future<User> getUser(String id) async {
    final response = await _apiClient.get('/users/$id');
    return User.fromJson(response.data);
  }
  
  @override
  Future<User> updateUser(User user) async {
    final response = await _apiClient.put('/users/${user.id}', user.toJson());
    return User.fromJson(response.data);
  }
  
  @override
  Future<void> deleteUser(String id) async {
    await _apiClient.delete('/users/$id');
  }
}

// BLoC using repository
class UserBloc extends Bloc<UserEvent, UserState> {
  final UserRepository _repository;
  
  UserBloc(this._repository) : super(UserInitial()) {
    on<LoadUser>(_onLoadUser);
  }
  
  Future<void> _onLoadUser(LoadUser event, Emitter<UserState> emit) async {
    emit(UserLoading());
    
    try {
      final user = await _repository.getUser(event.userId);
      emit(UserLoaded(user));
    } catch (e) {
      emit(UserError(e.toString()));
    }
  }
}
```

**3. Error Handling:**

```dart
class UserBloc extends Bloc<UserEvent, UserState> {
  final UserRepository _repository;
  
  UserBloc(this._repository) : super(UserInitial()) {
    on<LoadUser>(_onLoadUser);
  }
  
  Future<void> _onLoadUser(LoadUser event, Emitter<UserState> emit) async {
    emit(UserLoading());
    
    try {
      final user = await _repository.getUser(event.userId);
      emit(UserLoaded(user));
    } on NetworkException catch (e) {
      emit(UserError('Network error: ${e.message}'));
    } on ValidationException catch (e) {
      emit(UserError('Validation error: ${e.message}'));
    } catch (e) {
      emit(UserError('Unexpected error: $e'));
    }
  }
}
```

**4. Concurrency Handling:**

```dart
class UserBloc extends Bloc<UserEvent, UserState> {
  final UserRepository _repository;
  
  UserBloc(this._repository) : super(UserInitial()) {
    on<LoadUser>(
      _onLoadUser,
      transformer: restartable(), // Cancel previous requests
    );
    
    on<UpdateUser>(
      _onUpdateUser,
      transformer: droppable(), // Drop if already updating
    );
  }
  
  Future<void> _onLoadUser(LoadUser event, Emitter<UserState> emit) async {
    emit(UserLoading());
    
    try {
      final user = await _repository.getUser(event.userId);
      emit(UserLoaded(user));
    } catch (e) {
      emit(UserError(e.toString()));
    }
  }
  
  Future<void> _onUpdateUser(UpdateUser event, Emitter<UserState> emit) async {
    emit(UserUpdating());
    
    try {
      final updatedUser = await _repository.updateUser(event.user);
      emit(UserUpdated(updatedUser));
    } catch (e) {
      emit(UserError(e.toString()));
    }
  }
}
```

**5. Analytics Integration:**

```dart
class UserBloc extends Bloc<UserEvent, UserState> {
  final UserRepository _repository;
  final AnalyticsService _analytics;
  
  UserBloc(this._repository, this._analytics) : super(UserInitial()) {
    on<LoadUser>(_onLoadUser);
  }
  
  Future<void> _onLoadUser(LoadUser event, Emitter<UserState> emit) async {
    emit(UserLoading());
    
    // Track event
    _analytics.track('user_load_started', {'user_id': event.userId});
    
    try {
      final user = await _repository.getUser(event.userId);
      emit(UserLoaded(user));
      
      // Track success
      _analytics.track('user_load_success', {'user_id': event.userId});
    } catch (e) {
      emit(UserError(e.toString()));
      
      // Track error
      _analytics.track('user_load_error', {
        'user_id': event.userId,
        'error': e.toString(),
      });
    }
  }
}
```

**6. Caching Strategy:**

```dart
class UserBloc extends Bloc<UserEvent, UserState> {
  final UserRepository _repository;
  final CacheService _cache;
  
  UserBloc(this._repository, this._cache) : super(UserInitial()) {
    on<LoadUser>(_onLoadUser);
  }
  
  Future<void> _onLoadUser(LoadUser event, Emitter<UserState> emit) async {
    emit(UserLoading());
    
    try {
      // Try cache first
      final cachedUser = await _cache.getUser(event.userId);
      if (cachedUser != null) {
        emit(UserLoaded(cachedUser));
      }
      
      // Load from API
      final user = await _repository.getUser(event.userId);
      
      // Update cache
      await _cache.setUser(user);
      
      emit(UserLoaded(user));
    } catch (e) {
      emit(UserError(e.toString()));
    }
  }
}
```

**Side Effects-এর Best Practices:**

1. **Keep side effects in event handlers**: Don't put side effects in state
2. **Use repository pattern**: Abstract data access logic
3. **Handle errors properly**: Use try-catch blocks
4. **Use concurrency operators**: Prevent race conditions
5. **Track analytics**: Monitor user behavior
6. **Implement caching**: Improve performance

**Side Effects-এর Pitfalls:**

1. **Putting side effects in state**: State should be pure
2. **Not handling errors**: Always handle exceptions
3. **Race conditions**: Use concurrency operators
4. **Memory leaks**: Dispose resources properly
5. **Blocking UI**: Use async operations

**Interview Tips:** 
- Side effects event handler-এ রাখতে হবে
- Repository pattern ব্যবহার করা ভালো
- Error handling গুরুত্বপূর্ণ
- Concurrency operators ব্যবহার করুন
- Analytics tracking implement করুন

---

### প্রশ্ন ২৫: State persistence (app restart) কিভাবে করবেন?

**উত্তর (ডিটেইল):**

**State Persistence কী:**

State persistence হলো অ্যাপ restart হওয়ার পর state restore করার প্রক্রিয়া। এটি user experience improve করার জন্য খুবই গুরুত্বপূর্ণ।

**State Persistence-এর বিভিন্ন ধরন:**

1. **Local Storage**: SharedPreferences, Hive, SQLite
2. **Secure Storage**: Encrypted storage for sensitive data
3. **Cloud Storage**: Firebase, AWS, etc.
4. **Hybrid Approach**: Local + Cloud combination

**State Persistence Implementation:**

**1. SharedPreferences ব্যবহার:**

```dart
// State persistence service
class StatePersistenceService {
  static const String _userKey = 'user';
  static const String _themeKey = 'theme';
  static const String _languageKey = 'language';
  
  final SharedPreferences _prefs;
  
  StatePersistenceService(this._prefs);
  
  // Save user state
  Future<void> saveUser(User user) async {
    await _prefs.setString(_userKey, json.encode(user.toJson()));
  }
  
  // Load user state
  User? loadUser() {
    final userJson = _prefs.getString(_userKey);
    if (userJson != null) {
      return User.fromJson(json.decode(userJson));
    }
    return null;
  }
  
  // Save theme state
  Future<void> saveTheme(ThemeData theme) async {
    await _prefs.setString(_themeKey, theme.brightness.name);
  }
  
  // Load theme state
  ThemeData? loadTheme() {
    final themeName = _prefs.getString(_themeKey);
    if (themeName == 'dark') {
      return ThemeData.dark();
    } else if (themeName == 'light') {
      return ThemeData.light();
    }
    return null;
  }
  
  // Save language state
  Future<void> saveLanguage(String language) async {
    await _prefs.setString(_languageKey, language);
  }
  
  // Load language state
  String? loadLanguage() {
    return _prefs.getString(_languageKey);
  }
  
  // Clear all data
  Future<void> clearAll() async {
    await _prefs.clear();
  }
}

// Provider setup
final persistenceServiceProvider = Provider<StatePersistenceService>((ref) {
  return StatePersistenceService(SharedPreferences.getInstance());
});

// User provider with persistence
final userProvider = StateNotifierProvider<UserNotifier, UserState>((ref) {
  final persistenceService = ref.read(persistenceServiceProvider);
  return UserNotifier(persistenceService);
});

// User notifier with persistence
class UserNotifier extends StateNotifier<UserState> {
  final StatePersistenceService _persistenceService;
  
  UserNotifier(this._persistenceService) : super(UserState()) {
    _loadUser();
  }
  
  Future<void> _loadUser() async {
    final savedUser = _persistenceService.loadUser();
    if (savedUser != null) {
      state = UserState(user: savedUser);
    }
  }
  
  Future<void> updateUser(User user) async {
    state = UserState(user: user);
    await _persistenceService.saveUser(user);
  }
  
  Future<void> logout() async {
    state = UserState();
    await _persistenceService.clearAll();
  }
}
```

**2. Hive ব্যবহার (Recommended):**

```dart
// Initialize Hive
Future<void> initializeHive() async {
  await Hive.initFlutter();
  Hive.registerAdapter(UserAdapter());
  await Hive.openBox<User>('users');
  await Hive.openBox<Settings>('settings');
}

// User model with Hive
@HiveType(typeId: 0)
class User extends HiveObject {
  @HiveField(0)
  final String id;
  
  @HiveField(1)
  final String name;
  
  @HiveField(2)
  final String email;
  
  User({
    required this.id,
    required this.name,
    required this.email,
  });
}

// User adapter
class UserAdapter extends TypeAdapter<User> {
  @override
  final int typeId = 0;
  
  @override
  User read(BinaryReader reader) {
    return User(
      id: reader.readString(),
      name: reader.readString(),
      email: reader.readString(),
    );
  }
  
  @override
  void write(BinaryWriter writer, User obj) {
    writer.writeString(obj.id);
    writer.writeString(obj.name);
    writer.writeString(obj.email);
  }
}

// State persistence with Hive
class HivePersistenceService {
  static const String _userBox = 'users';
  static const String _settingsBox = 'settings';
  
  Future<void> saveUser(User user) async {
    final box = Hive.box<User>(_userBox);
    await box.put(user.id, user);
  }
  
  User? loadUser(String id) {
    final box = Hive.box<User>(_userBox);
    return box.get(id);
  }
  
  Future<void> saveSettings(Settings settings) async {
    final box = Hive.box<Settings>(_settingsBox);
    await box.put('settings', settings);
  }
  
  Settings? loadSettings() {
    final box = Hive.box<Settings>(_settingsBox);
    return box.get('settings');
  }
  
  Future<void> clearAll() async {
    await Hive.box<User>(_userBox).clear();
    await Hive.box<Settings>(_settingsBox).clear();
  }
}
```

**3. Riverpod-এ State Persistence:**

```dart
// SharedPreferences provider
final sharedPreferencesProvider = Provider<SharedPreferences>((ref) {
  throw UnimplementedError();
});

// Initialize in main
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  final prefs = await SharedPreferences.getInstance();
  
  runApp(
    ProviderScope(
      overrides: [
        sharedPreferencesProvider.overrideWithValue(prefs),
      ],
      child: MyApp(),
    ),
  );
}

// User provider with persistence
final userProvider = StateNotifierProvider<UserNotifier, UserState>((ref) {
  final prefs = ref.read(sharedPreferencesProvider);
  return UserNotifier(prefs);
});

// User notifier
class UserNotifier extends StateNotifier<UserState> {
  final SharedPreferences _prefs;
  
  UserNotifier(this._prefs) : super(UserState()) {
    _loadUser();
  }
  
  Future<void> _loadUser() async {
    final userJson = _prefs.getString('user');
    if (userJson != null) {
      final user = User.fromJson(json.decode(userJson));
      state = UserState(user: user);
    }
  }
  
  Future<void> updateUser(User user) async {
    state = UserState(user: user);
    await _prefs.setString('user', json.encode(user.toJson()));
  }
  
  Future<void> logout() async {
    state = UserState();
    await _prefs.remove('user');
  }
}
```

**4. BLoC-এ State Persistence (hydrated_bloc):**

```dart
// Initialize hydrated_bloc
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  HydratedBloc.storage = await HydratedStorage.build(
    storageDirectory: await getTemporaryDirectory(),
  );
  
  runApp(MyApp());
}

// User state with persistence
class UserState extends Equatable {
  final User? user;
  final bool isLoading;
  final String? error;
  
  const UserState({
    this.user,
    this.isLoading = false,
    this.error,
  });
  
  UserState copyWith({
    User? user,
    bool? isLoading,
    String? error,
  }) {
    return UserState(
      user: user ?? this.user,
      isLoading: isLoading ?? this.isLoading,
      error: error ?? this.error,
    );
  }
  
  @override
  List<Object?> get props => [user, isLoading, error];
  
  // JSON serialization for persistence
  Map<String, dynamic> toJson() {
    return {
      'user': user?.toJson(),
      'isLoading': isLoading,
      'error': error,
    };
  }
  
  factory UserState.fromJson(Map<String, dynamic> json) {
    return UserState(
      user: json['user'] != null ? User.fromJson(json['user']) : null,
      isLoading: json['isLoading'] ?? false,
      error: json['error'],
    );
  }
}

// User BLoC with persistence
class UserBloc extends HydratedBloc<UserEvent, UserState> {
  final UserRepository _repository;
  
  UserBloc(this._repository) : super(const UserState()) {
    on<LoadUser>(_onLoadUser);
    on<UpdateUser>(_onUpdateUser);
    on<Logout>(_onLogout);
  }
  
  Future<void> _onLoadUser(LoadUser event, Emitter<UserState> emit) async {
    emit(state.copyWith(isLoading: true));
    
    try {
      final user = await _repository.getUser(event.userId);
      emit(state.copyWith(user: user, isLoading: false));
    } catch (e) {
      emit(state.copyWith(error: e.toString(), isLoading: false));
    }
  }
  
  Future<void> _onUpdateUser(UpdateUser event, Emitter<UserState> emit) async {
    emit(state.copyWith(isLoading: true));
    
    try {
      final updatedUser = await _repository.updateUser(event.user);
      emit(state.copyWith(user: updatedUser, isLoading: false));
    } catch (e) {
      emit(state.copyWith(error: e.toString(), isLoading: false));
    }
  }
  
  Future<void> _onLogout(Logout event, Emitter<UserState> emit) async {
    emit(const UserState());
  }
  
  @override
  UserState? fromJson(Map<String, dynamic> json) {
    return UserState.fromJson(json);
  }
  
  @override
  Map<String, dynamic>? toJson(UserState state) {
    return state.toJson();
  }
}
```

**5. Secure Storage (Sensitive Data):**

```dart
// Secure storage service
class SecureStorageService {
  final FlutterSecureStorage _storage;
  
  SecureStorageService(this._storage);
  
  // Save sensitive data
  Future<void> saveToken(String token) async {
    await _storage.write(key: 'auth_token', value: token);
  }
  
  // Load sensitive data
  Future<String?> loadToken() async {
    return await _storage.read(key: 'auth_token');
  }
  
  // Save user credentials
  Future<void> saveCredentials(String username, String password) async {
    await _storage.write(key: 'username', value: username);
    await _storage.write(key: 'password', value: password);
  }
  
  // Load user credentials
  Future<Map<String, String?>> loadCredentials() async {
    final username = await _storage.read(key: 'username');
    final password = await _storage.read(key: 'password');
    return {'username': username, 'password': password};
  }
  
  // Clear sensitive data
  Future<void> clearSensitiveData() async {
    await _storage.deleteAll();
  }
}

// Provider setup
final secureStorageProvider = Provider<SecureStorageService>((ref) {
  return SecureStorageService(const FlutterSecureStorage());
});

// Auth provider with secure storage
final authProvider = StateNotifierProvider<AuthNotifier, AuthState>((ref) {
  final secureStorage = ref.read(secureStorageProvider);
  return AuthNotifier(secureStorage);
});
```

**State Persistence-এর Best Practices:**

1. **Choose appropriate storage**: Use right storage for data type
2. **Handle errors gracefully**: Always handle storage errors
3. **Implement migration**: Handle data format changes
4. **Secure sensitive data**: Use secure storage for sensitive data
5. **Optimize performance**: Don't save too frequently
6. **Test thoroughly**: Test persistence in different scenarios

**State Persistence-এর Pitfalls:**

1. **Not handling errors**: Storage operations can fail
2. **Saving too frequently**: Performance impact
3. **Not securing sensitive data**: Security risks
4. **Not implementing migration**: Data format changes
5. **Memory leaks**: Not disposing resources

**Interview Tips:** 
- State persistence user experience improve করে
- Sensitive data secure storage-এ রাখতে হবে
- Error handling গুরুত্বপূর্ণ
- Performance optimization করতে হবে
- Testing-এ persistence properly test করতে হবে


