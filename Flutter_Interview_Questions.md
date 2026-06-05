# Flutter Interview Questions - Easy English Answers

Use this file for interview preparation. Each answer is written in simple English so you can understand it, memorize it, and explain it naturally.

Interview tip: do not try to speak every line. Start with the short answer, then add 2 or 3 key points and one real example from your project.

---

## State Management

### 1. What state management techniques have you worked with in Flutter?
**Short answer:** I have worked with `setState`, Provider, Riverpod, Bloc, and GetX.

**Interview answer:** For small local UI changes, I use `setState`. For shared app state, I use Provider or Riverpod. For large applications with complex flows, I prefer Bloc because it gives a clear event-to-state pattern. I have also used GetX where the project needed fast development with less boilerplate.

**Remember:** Local state = `setState`; shared state = Provider/Riverpod; complex business flow = Bloc; fast/simple controller style = GetX.

### 2. Why would you choose GetX over Provider or Bloc?
**Short answer:** I choose GetX when I need quick development, simple syntax, routing, dependency injection, and reactive state in one package.

**Interview answer:** GetX reduces boilerplate. I can create controllers, reactive variables, routes, and dependencies without passing `BuildContext`. But I use it carefully because if the team does not follow structure, the code can become tightly coupled.

**Good line to say:** GetX is fast and simple, but for very large teams I prefer Bloc or Riverpod for better structure and testability.

### 3. What are the advantages and disadvantages of GetX?
**Advantages:**
- Less boilerplate.
- No need to pass `BuildContext` for navigation or state access.
- Provides state management, routing, and dependency injection.
- Reactive UI updates using `.obs` and `Obx`.

**Disadvantages:**
- Easy to misuse in large projects.
- Can make business logic and UI tightly connected.
- Testing can become harder if controllers are not written cleanly.
- Too much global access can make debugging difficult.

### 4. What is the difference between Provider, Riverpod, Bloc, and GetX?
**Provider:** Simple and good for small to medium apps. It depends on `BuildContext`.

**Riverpod:** Improved Provider. It does not depend on `BuildContext`, is easier to test, and handles async state nicely.

**Bloc:** Best for complex apps. It uses Events and States, so data flow is predictable and easy to test.

**GetX:** Simple and fast. It uses controllers and reactive variables, but needs discipline in large projects.

**Memory line:** Provider is simple, Riverpod is safer, Bloc is strict, GetX is fast.

### 5. Why is Riverpod considered an improvement over Provider?
Riverpod solves common Provider problems. It does not need `BuildContext`, so we can read providers outside widgets. It gives better compile-time safety, better testing support, and built-in support for `FutureProvider` and `StreamProvider`.

### 6. What is the difference between reactive and non-reactive state management in GetX?
**Reactive GetX:** Uses `.obs` variables and `Obx`. UI updates automatically when the value changes.

```dart
final count = 0.obs;
Obx(() => Text('${count.value}'));
```

**Non-reactive GetX:** Uses normal variables, `GetBuilder`, and manual `update()`.

```dart
int count = 0;
void increment() {
  count++;
  update();
}
```

### 7. Explain Rx variables in GetX.
Rx variables are observable variables. When their value changes, only the widgets listening to them rebuild.

Example:

```dart
var name = 'Amit'.obs;
name.value = 'Rahul';
```

Here `name` is reactive. Any `Obx` widget using `name.value` will update automatically.

### 8. What is the purpose of notifyListeners() in Provider?
`notifyListeners()` tells all listening widgets that data has changed. After this call, widgets using `Consumer`, `Provider.of`, or `Selector` can rebuild with the latest value.

### 9. When would you use Bloc instead of GetX?
I use Bloc when the app has complex business rules, multiple developers, many states, and needs strong testing. Bloc makes state changes predictable because every UI change comes from an event and produces a state.

### 10. How does state persist after app refresh or app restart?
Normal in-memory state is lost after restart. To persist state, I save important data in local storage:

- `shared_preferences` for small non-sensitive values.
- `flutter_secure_storage` for tokens.
- Hive or SQLite for larger local data.

On app start, I read the saved data and restore the state.

---

## Flutter Architecture

### 11. Which architecture pattern have you used in Flutter projects?
I have used feature-based architecture, MVVM, and Clean Architecture. For large apps, I prefer feature-based Clean Architecture because each feature has its own presentation, domain, and data layers.

### 12. Explain Clean Architecture in Flutter.
Clean Architecture separates the app into layers:

- **Presentation:** UI, screens, widgets, Bloc/ViewModel/Provider.
- **Domain:** business rules, entities, use cases, repository contracts.
- **Data:** API calls, local database, models, repository implementation.

The main idea is that business logic should not depend on Flutter UI, API packages, or database packages.

### 13. What are the responsibilities of Presentation, Domain, and Data layers?
**Presentation:** Shows UI and handles user actions.

**Domain:** Contains pure business logic. It should be simple Dart code.

**Data:** Fetches and stores data using APIs, local database, cache, and models.

### 14. What are Use Cases in Clean Architecture?
A use case is one business action of the app. For example: `LoginUser`, `GetProducts`, `AddToCart`, or `SubmitOrder`.

The UI should not directly call APIs. The UI calls a use case, and the use case decides what business logic should run.

### 15. What is the role of Repository classes?
A repository hides where data comes from. The UI or domain layer only asks for data; it does not care whether the data comes from API, cache, or local database.

Example: `ProductRepository.getProducts()` may return cached products first and then update from API.

### 16. How do layers communicate with each other?
The flow is:

`UI -> State Manager -> Use Case -> Repository Interface -> Repository Implementation -> API/Database`

Data comes back in the opposite direction as entities or state objects.

### 17. How would you structure a Flutter project with Login, Dashboard, and Profile?
```text
lib/
  core/
    network/
    storage/
    theme/
    utils/
  features/
    login/
      data/
      domain/
      presentation/
    dashboard/
      data/
      domain/
      presentation/
    profile/
      data/
      domain/
      presentation/
```

### 18. Where would you place Providers, Bloc classes, APIs, Models, and Repositories?
- Providers/Bloc/ViewModel: `presentation`
- Use cases and repository interfaces: `domain`
- API services, DTO models, and repository implementations: `data`
- Common utilities: `core`

### 19. What are the benefits of feature-based folder structure?
It keeps related files together. A developer can work on login, dashboard, or profile without searching across the whole project. It also makes the project easier to scale, test, and maintain.

### 20. How do you maintain scalability in large Flutter applications?
I maintain scalability by using feature modules, clean architecture, dependency injection, reusable widgets, proper state management, API abstraction, tests, and clear code review rules.

---

## Navigation & Session Handling

### 21. How do you manage navigation in Flutter?
I use Flutter `Navigator` for simple apps and `go_router` for large apps with deep links, web URLs, authentication redirects, and nested navigation.

### 22. What is the difference between Navigator and GetX navigation?
`Navigator` is Flutter's built-in navigation system and usually uses `BuildContext`. GetX navigation uses `Get.to`, `Get.off`, and `Get.back`, and can navigate without passing `BuildContext`.

### 23. How do you restore navigation state after refresh?
In Flutter Web, I use URL-based routing like `go_router`. When the browser refreshes, the app reads the current URL and opens the correct screen again.

### 24. How do you persist session data in Flutter?
I store access tokens or refresh tokens in secure storage. I store non-sensitive session data, like selected language or theme, in shared preferences. On app launch, I check the token and decide whether to show login or home.

### 25. What happens when a user refreshes a Flutter web application?
The app restarts and all memory state is lost. So route information should come from the URL, and important data should be restored from browser storage or backend APIs.

### 26. How would you save the last visited screen?
I can save the route path whenever navigation changes. On the next app start, I read the saved route and redirect the user to that screen if the session is still valid.

---

## Async Programming

### 27. What is the difference between FutureBuilder and StreamBuilder?
`FutureBuilder` handles one future result, like one API call.

`StreamBuilder` listens to many values over time, like chat messages, live location, WebSocket data, or Firestore snapshots.

### 28. What is the difference between BlocBuilder and FutureBuilder?
`FutureBuilder` is for simple one-time async UI. `BlocBuilder` is for state-driven UI where events, loading, success, error, and business logic are handled outside the widget.

### 29. When should you use FutureBuilder?
Use `FutureBuilder` for simple one-time tasks like loading settings or fetching simple data for one screen. For complex business logic, use Bloc, Provider, or Riverpod.

### 30. When should you use StreamBuilder?
Use `StreamBuilder` when data changes continuously, like real-time chat, live notifications, location updates, or database streams.

### 31. How do you handle API loading, success, and error states?
I create clear states:

- Loading: show progress indicator or shimmer.
- Success: show data.
- Empty: show empty message.
- Error: show message and retry button.

This avoids broken screens and improves user experience.

---

## Flutter Web

### 32. Have you worked on Flutter Web projects?
Yes. Flutter Web lets us run Flutter apps in the browser. I handle responsive UI, browser refresh, URL routing, file upload differences, and package compatibility.

### 33. What are the challenges in Flutter Web?
Common challenges are large initial load size, SEO limitations, browser refresh handling, responsive layouts for large screens, and packages that only support Android/iOS.

### 34. How do you make Flutter Web responsive?
I use `LayoutBuilder`, `MediaQuery`, `Flexible`, `Expanded`, `Wrap`, and breakpoints. For example, mobile can show one column, tablet two columns, and desktop a wider layout with side navigation.

### 35. What is the difference between MediaQuery and LayoutBuilder?
`MediaQuery` gives full screen size. `LayoutBuilder` gives the size available to a particular widget from its parent.

**Memory line:** MediaQuery = device/screen size. LayoutBuilder = parent constraint size.

### 36. How do you optimize Flutter Web performance?
I reduce asset size, use lazy loading, avoid heavy work on startup, use caching, remove unused packages, optimize images, and keep rebuilds small.

### 37. How do you handle browser refresh in Flutter Web?
I use URL-based routing and store important data like token or selected workspace in local storage or secure backend session. After refresh, the app rebuilds state from URL plus stored data.

---

## File Upload & Platform Handling

### 38. How do you upload files in Flutter?
I use `file_picker` or `image_picker` to select the file, then use `dio` or `http` to send it as `multipart/form-data`.

### 39. How do you upload files in Flutter Web?
On web, we usually do not get a normal file path. We read file bytes and upload using `MultipartFile.fromBytes`.

### 40. What is multipart/form-data?
It is a request format used to send files and normal text fields together in one API request.

### 41. What packages have you used for file upload?
I have used `file_picker`, `image_picker`, `dio`, and sometimes `http`.

### 42. How do you handle platform-specific implementations in Flutter?
I use conditional imports, `kIsWeb`, `Platform.isAndroid`, `Platform.isIOS`, or platform channels when I need native Android/iOS code.

### 43. What is kIsWeb in Flutter?
`kIsWeb` is a Flutter constant that tells whether the app is running on the web.

---

## Dependency & Deployment

### 44. What happens when a new package dependency is added?
We add it to `pubspec.yaml` and run `flutter pub get`. Flutter downloads the package and updates `pubspec.lock` with exact versions.

### 45. What issues can occur after deploying to production?
Production issues can include crashes, API failures, slow loading, layout overflow on some devices, permission issues, and OS-specific bugs.

### 46. How do you manage package versions in Flutter?
I use proper version constraints in `pubspec.yaml`, commit `pubspec.lock` for apps, regularly update packages, and test after updates.

### 47. What is dependency conflict?
A dependency conflict happens when two packages need incompatible versions of another package. We solve it by upgrading packages, changing versions, or using `dependency_overrides` carefully.

### 48. How do you debug production issues?
I use Firebase Crashlytics, Sentry, logs, analytics, release build testing, and stack traces. I also check device model, OS version, app version, and user steps.

---

## Flutter Fundamentals

### 49. What is the widget lifecycle in Flutter?
For a `StatefulWidget`, the common lifecycle is:

`createState -> initState -> didChangeDependencies -> build -> didUpdateWidget -> deactivate -> dispose`

Use `initState` for one-time setup and `dispose` to clean controllers, streams, and listeners.

### 50. Difference between StatelessWidget and StatefulWidget?
`StatelessWidget` has no internal mutable state. It is good for static UI.

`StatefulWidget` has a `State` object and can update UI using `setState` or state management.

### 51. What are Keys in Flutter?
Keys help Flutter identify widgets, especially in lists or when widgets move position. They help preserve the correct state for the correct widget.

### 52. What is BuildContext?
`BuildContext` represents the location of a widget in the widget tree. We use it to access theme, media query, navigator, provider, and parent widgets.

### 53. What causes unnecessary widget rebuilds?
Common reasons are calling `setState` too high in the tree, creating futures inside `build`, not using `const`, listening to full state instead of selected state, and putting heavy logic inside `build`.

### 54. How do you optimize Flutter app performance?
Use `const`, keep rebuild areas small, use `ListView.builder`, cache images, avoid heavy work in `build`, use DevTools, add `RepaintBoundary` where needed, and move CPU-heavy work to isolates.

### 55. What are mixins in Dart?
Mixins allow a class to reuse methods from another class without normal inheritance.

```dart
mixin Logger {
  void log(String message) => print(message);
}

class UserService with Logger {}
```

### 56. What are isolates in Flutter?
Isolates are separate Dart workers with their own memory. We use them for CPU-heavy tasks so the main UI thread does not freeze.

---

## Practical & Scenario-Based Questions

### 57. How would you design a scalable e-commerce Flutter app?
I would divide it into features like auth, product, cart, checkout, orders, and profile. I would use Clean Architecture, Bloc/Riverpod, Dio for APIs, secure storage for tokens, local caching for products/cart, and proper error handling.

### 58. Suppose the API fails, how would your UI behave?
The UI should show a friendly error state, not a blank screen. It should show retry, cached data if available, and clear messages like "Something went wrong" or "No internet connection".

### 59. How would you implement offline support?
I would store important data locally using Hive or SQLite. The app reads from local storage first, then syncs with the server when internet is available.

### 60. How would you handle app state after internet reconnect?
I would listen to connectivity changes. When internet comes back, I would sync pending local actions, refresh stale data, and update UI state.

### 61. How would you secure sensitive data in Flutter?
I would use HTTPS, secure storage for tokens, encrypted local database for sensitive data, avoid hardcoding secrets, use obfuscation for release, and consider SSL pinning for high-security apps.

### 62. How would you handle deep linking?
I would configure Android app links and iOS universal links, then use `go_router` to parse the incoming URL and navigate to the correct screen.

### 63. Explain a challenging bug you solved recently.
Use this format:

**Problem:** The app was crashing or behaving incorrectly.

**Reason:** After debugging, I found the root cause.

**Fix:** I changed the code and verified it.

**Result:** The crash/performance issue was resolved.

Example answer: I fixed a memory leak caused by not disposing controllers and stream subscriptions. I added proper cleanup in `dispose`, tested navigation multiple times, and confirmed memory was stable in DevTools.

### 64. Explain a performance issue you faced in Flutter and how you fixed it.
Example answer: A long list was lagging because many widgets were rebuilding. I used Flutter DevTools, found the rebuild area, added `const`, used `ListView.builder`, reduced expensive widgets, and moved heavy parsing outside the UI thread.

### 65. If your app crashes only in production, how would you debug it?
I would check Crashlytics/Sentry logs, reproduce with a release build, check device-specific details, verify API responses, and use symbolicated stack traces if obfuscation is enabled.

---

## Team & Project Experience

### 66. How do you collaborate with backend teams?
I discuss API contracts, request/response JSON, status codes, error format, authentication, pagination, and edge cases before implementation. I prefer Swagger/OpenAPI or Postman collections.

### 67. How do you manage code reviews?
I check readability, architecture, naming, error handling, performance, security, and tests. I also make sure code follows project conventions.

### 68. Have you worked in Agile/Scrum?
Yes. I have worked with sprint planning, daily standups, task estimation, Jira boards, sprint reviews, and retrospectives.

### 69. How do you handle merge conflicts?
I first understand both changes, then resolve the conflict carefully, run formatting and tests, and confirm the feature still works. If needed, I talk to the developer who changed the same code.

### 70. What role do you usually play in your project team?
I usually work as a Flutter developer who handles feature development, API integration, state management, code quality, bug fixing, and sometimes mentoring junior developers.

---

## API & Networking

### 71. What is the difference between REST API and GraphQL?
REST has multiple endpoints and each endpoint returns a fixed response. GraphQL usually has one endpoint and the client asks for exactly the fields it needs.

### 72. How do you handle multiple API calls simultaneously in Flutter?
I use `Future.wait` when calls are independent.

```dart
final results = await Future.wait([
  api.getProfile(),
  api.getOrders(),
  api.getNotifications(),
]);
```

### 73. What is Dio in Flutter and why is it preferred over HTTP package?
Dio is an advanced HTTP client. It supports interceptors, timeout, cancellation, file upload/download, progress callbacks, global configuration, and better error handling.

### 74. What are interceptors in Dio?
Interceptors are hooks that run before request, after response, or on error. They are useful for adding tokens, logging, handling errors, and refreshing tokens.

### 75. Explain onRequest, onResponse, and onError in Dio interceptors.
- `onRequest`: before sending request. Add headers/token.
- `onResponse`: after successful response. Log or transform response.
- `onError`: when request fails. Handle timeout, 401, retry, or logout.

### 76. How do you add authentication tokens using interceptors?
In `onRequest`, read the token from secure storage and add it to the header:

```dart
options.headers['Authorization'] = 'Bearer $token';
```

### 77. How do you refresh expired JWT tokens automatically?
When API returns 401, I call refresh-token API, save the new access token, update the failed request header, and retry the original request. If refresh fails, I logout the user.

### 78. What is global error handling in Flutter networking?
It means handling common API errors in one place, like timeout, no internet, 401, 500, and validation errors. This keeps UI code cleaner and gives consistent error messages.

### 79. How do you handle API timeout scenarios?
I set connect, send, and receive timeouts in Dio. If timeout happens, I catch the exception and show a retry option.

### 80. How do you implement retry mechanisms for failed APIs?
I retry only safe requests and temporary failures like timeout or network error. I use a retry count and delay between attempts.

### 81. What is middleware? How is it different from interceptors?
Middleware usually works at app routing/request flow level, like checking auth before opening a route. Interceptors work at network level, like modifying HTTP requests and responses.

### 82. Where are middleware commonly used?
Middleware is commonly used for authentication checks, role-based access, logging navigation, and redirecting users.

### 83. Can interceptors modify request headers and responses?
Yes. Interceptors can add headers, modify request data, transform response data, and handle errors globally.

### 84. How do you log API requests and responses in Flutter?
I use Dio interceptors or packages like `pretty_dio_logger` in debug mode. I avoid logging sensitive data like passwords and tokens.

### 85. What is request cancellation in Dio?
Request cancellation stops an active API call using `CancelToken`. It is useful when the user leaves the screen or starts a new search.

### 86. How do you upload files/images using Dio?
I create `FormData`, add `MultipartFile`, and send it using `dio.post`.

### 87. How do you download large files efficiently in Flutter?
I use `dio.download`, save directly to file path, show progress using `onReceiveProgress`, and handle cancellation/resume if required.

### 88. How do you secure API calls in Flutter applications?
Use HTTPS, token-based auth, secure token storage, avoid hardcoded secrets, validate certificates where needed, and handle session expiration globally.

---

## Additional State Management

### 89. Which state management approaches have you used in Flutter?
I have used `setState`, Provider, Riverpod, Bloc, and GetX. I choose based on project size and complexity.

### 90. Difference between Provider, Bloc, Riverpod, and GetX?
Provider is simple. Bloc is strict and testable. Riverpod is safer and does not need context. GetX is fast and less boilerplate.

### 91. What is the difference between setState and Provider?
`setState` is for local state inside one widget. Provider is for shared state used by multiple widgets or screens.

### 92. When would you choose Bloc over Provider?
I choose Bloc when the flow is complex, states are many, business logic must be tested, or multiple developers are working on the same feature.

### 93. Explain reactive programming in Flutter.
Reactive programming means UI reacts automatically when data changes. The UI is built from state, and when state changes, the UI updates.

### 94. What are streams in Dart?
A stream is a sequence of asynchronous events. It can emit multiple values over time, like chat messages or location updates.

### 95. Difference between StreamBuilder and FutureBuilder?
`FutureBuilder` handles one result. `StreamBuilder` handles many results over time.

---

## Flutter Core Concepts

### 96. Difference between StatelessWidget and StatefulWidget?
StatelessWidget is immutable and does not manage internal state. StatefulWidget has a `State` object and can rebuild when its state changes.

### 97. What is widget lifecycle in Flutter?
It is the journey of a StatefulWidget from creation to removal: `initState`, `build`, update methods, and `dispose`.

### 98. Difference between initState(), build(), and dispose()?
`initState` runs once for setup. `build` creates UI and can run many times. `dispose` runs once for cleanup.

### 99. What are keys in Flutter?
Keys help Flutter match widgets correctly during rebuilds, especially in lists and reordered widgets.

### 100. Difference between GlobalKey and ValueKey?
`ValueKey` identifies widgets using a value, commonly in lists. `GlobalKey` is unique across the app and can access widget state, but it is heavier and should be used carefully.

### 101. What is BuildContext?
BuildContext is the position of a widget in the widget tree. It helps find theme, navigator, providers, and parent widgets.

### 102. What causes widget rebuilds?
`setState`, parent rebuilds, provider/state changes, stream updates, future completion, or inherited widget changes can cause rebuilds.

### 103. How do you optimize Flutter app performance?
Use `const`, reduce rebuilds, use builders for lists, avoid heavy work in UI, optimize images, profile with DevTools, and use isolates for heavy CPU tasks.

### 104. What is const constructor optimization?
`const` widgets are created at compile time. Flutter can reuse them, which reduces unnecessary rebuild work.

### 105. What are mixins in Dart?
Mixins let a class use reusable methods without extending a base class.

### 106. Difference between final and const?
`final` value is assigned once at runtime. `const` value must be known at compile time.

### 107. What are extensions in Dart?
Extensions add new methods or getters to existing classes without changing the original class.

```dart
extension StringExt on String {
  bool get isValidEmail => contains('@');
}
```

---

## Isolates & Performance

### 108. What are isolates in Flutter?
Isolates are separate Dart execution units. They do not share memory and communicate using messages.

### 109. Why do we use isolates?
We use isolates to run heavy CPU work without blocking the main UI thread.

### 110. Difference between async/await and isolates?
`async/await` makes waiting non-blocking, but it still runs on the same isolate. Isolates run code in parallel on a separate worker.

### 111. What tasks should be moved to isolates?
Large JSON parsing, image processing, encryption, file compression, and heavy calculations.

### 112. Explain compute() in Flutter.
`compute()` is a helper that runs a function in another isolate and returns the result.

### 113. How do isolates communicate with each other?
They communicate using `SendPort` and `ReceivePort` because they do not share memory.

### 114. What happens if heavy processing runs on the main thread?
The UI freezes, frames drop, animations lag, and the app feels slow.

### 115. Difference between concurrency and parallelism?
Concurrency means managing multiple tasks at the same time. Parallelism means actually running multiple tasks at the same time on different CPU cores.

---

## Local Storage & Caching

### 116. Difference between SharedPreferences, Hive, and SQLite?
`SharedPreferences` is for small key-value data. Hive is fast NoSQL local storage. SQLite is relational storage for structured tables and complex queries.

### 117. How do you cache API responses?
I save API response data locally with a timestamp. Then I show cached data quickly and refresh it from the network when needed.

### 118. What is offline-first architecture?
Offline-first means the app works mainly from local storage. The server sync happens in the background when internet is available.

### 119. How do you store sensitive data securely in Flutter?
I store tokens in `flutter_secure_storage` and encrypt sensitive local databases.

### 120. What is Flutter Secure Storage?
It is a package that stores sensitive data using Android Keystore and iOS Keychain.

---

## Architecture Patterns

### 121. Which architecture patterns have you used?
I have used MVC, MVVM, and Clean Architecture. For large Flutter apps, I prefer MVVM or Clean Architecture.

### 122. MVC
MVC means Model, View, Controller. Model holds data, View shows UI, and Controller handles user actions and updates data.

### 123. MVVM
MVVM means Model, View, ViewModel. ViewModel prepares data and state for the UI. It is good with Provider, Riverpod, or GetX controllers.

### 124. Clean Architecture
Clean Architecture separates code into presentation, domain, and data layers so business rules stay independent.

### 125. Explain Clean Architecture in Flutter.
Presentation has UI and state manager, domain has entities/use cases/repository contracts, and data has API/database/repository implementations.

### 126. What is repository pattern?
Repository pattern provides one clean interface for data. It hides API, cache, and database details from the rest of the app.

### 127. What is dependency injection?
Dependency injection means passing required objects from outside instead of creating them inside a class. This makes code testable and loosely coupled.

### 128. Have you used GetIt or Injectable?
Yes. I use `get_it` for dependency registration and `injectable` to generate registration code automatically.

### 129. Difference between tightly coupled and loosely coupled code?
Tightly coupled code directly depends on concrete classes. Loosely coupled code depends on abstractions, so we can replace implementations easily.

---

## Firebase

### 130. Have you integrated Firebase in Flutter?
Yes. I have used Firebase Authentication, Firestore, Cloud Messaging, Crashlytics, Analytics, and Remote Config.

### 131. Difference between Firebase Authentication and Firestore?
Firebase Authentication manages login and user identity. Firestore stores app data in NoSQL documents and collections.

### 132. How do push notifications work in Flutter?
The app gets an FCM token, sends it to backend, backend sends notification to FCM, and FCM delivers it to the device.

### 133. What is Firebase Cloud Messaging (FCM)?
FCM is Google's service for sending push notifications and data messages to Android, iOS, and Web apps.

### 134. How do you handle background notifications?
I register a top-level background message handler and configure platform notification permissions. For terminated/background state, the OS handles delivery and Flutter processes the payload when allowed.

---

## Advanced Flutter

### 135. What are platform channels?
Platform channels allow Dart code to call native Android/iOS code.

### 136. How does Flutter communicate with native Android/iOS code?
Flutter uses `MethodChannel` for method calls and `EventChannel` for continuous native events.

### 137. Difference between Future and Stream?
Future gives one async value. Stream gives multiple async values over time.

### 138. What is code splitting?
Code splitting means loading some code later instead of loading everything at app startup. In Flutter it is commonly done with deferred imports.

### 139. What is tree shaking in Flutter?
Tree shaking removes unused code during release build, reducing app size.

### 140. What are flavors/environments in Flutter?
Flavors are separate build configurations like dev, staging, and production. They can use different app names, bundle IDs, API URLs, and keys.

### 141. How do you manage dev, staging, and production builds?
I use flavors and `--dart-define` to pass environment values like base URL, app name, and feature flags.

### 142. What are deep links?
Deep links are URLs that open a specific screen inside the app.

### 143. What is app lifecycle in Flutter?
App lifecycle states include `resumed`, `inactive`, `paused`, `detached`, and sometimes `hidden`. We observe them using `WidgetsBindingObserver`.

---

## Practical Scenarios

### 144. How would you design a dashboard with 5 simultaneous API calls?
If calls are independent, I use `Future.wait`. I show loading, handle partial/error states if needed, cache important data, and avoid calling APIs again on every rebuild.

### 145. How would you avoid duplicate API calls during screen rebuild?
I start the API call in `initState`, Bloc event, Riverpod provider, or ViewModel. I do not create the future directly inside `build`.

### 146. How would you optimize a slow Flutter app?
I profile with DevTools, check rebuilds and frame time, reduce widget rebuilds, optimize images, use lazy lists, move heavy work to isolates, and remove unnecessary work from startup.

### 147. How would you handle session expiration globally?
I handle 401 in a Dio interceptor. If refresh token works, retry the request. If refresh fails, clear session and redirect to login.

### 148. How would you implement pagination?
I use `ScrollController` or paging package. When the user reaches near the bottom, I fetch the next page, append results, and prevent duplicate loading using an `isLoading` flag.

### 149. How would you implement search with debounce?
I use a `Timer`. On every text change, cancel the previous timer and start a new one. When the user stops typing for 400-500 ms, call the search API.

### 150. How would you handle memory leaks in Flutter?
I dispose `TextEditingController`, `ScrollController`, `AnimationController`, `StreamSubscription`, and `Timer`. I also avoid keeping unnecessary references to `BuildContext`.

### 151. How would you improve app startup time?
I reduce work in `main`, lazy-load non-critical services, optimize assets, avoid blocking API calls before first screen, and initialize only what is required.

### 152. How would you debug a crashing Flutter app?
I check logs, reproduce the issue, run in debug/profile/release mode, inspect Crashlytics/Sentry, and identify the exact screen and user action.

### 153. How would you secure a production Flutter application?
Use HTTPS, secure storage, obfuscation, encrypted storage, backend validation, no hardcoded secrets, proper permission handling, and certificate pinning for sensitive apps.

---

## Coding Questions

### 154. Write a Dio interceptor example.
```dart
dio.interceptors.add(
  InterceptorsWrapper(
    onRequest: (options, handler) {
      options.headers['Authorization'] = 'Bearer token';
      handler.next(options);
    },
    onError: (error, handler) {
      print(error.message);
      handler.next(error);
    },
  ),
);
```

**Explain:** This interceptor adds a token before every request and handles errors in one place.

### 155. Write a simple Provider example.
```dart
class CounterProvider extends ChangeNotifier {
  int count = 0;

  void increment() {
    count++;
    notifyListeners();
  }
}
```

**Explain:** `notifyListeners()` updates all widgets listening to this provider.

### 156. Write pagination logic in Flutter.
```dart
final controller = ScrollController();
bool isLoading = false;
int page = 1;

void initPagination() {
  controller.addListener(() {
    final nearBottom = controller.position.pixels >=
        controller.position.maxScrollExtent - 200;

    if (nearBottom && !isLoading) {
      loadNextPage();
    }
  });
}

Future<void> loadNextPage() async {
  isLoading = true;
  page++;
  // call API and append data
  isLoading = false;
}
```

### 157. Write a debounce search implementation.
```dart
Timer? _debounce;

void onSearchChanged(String value) {
  _debounce?.cancel();
  _debounce = Timer(const Duration(milliseconds: 500), () {
    // call search API
  });
}
```

### 158. Write isolate example using compute().
```dart
import 'package:flutter/foundation.dart';

int parseData(String raw) {
  return raw.length;
}

Future<void> runParsing() async {
  final result = await compute(parseData, 'large data');
  print(result);
}
```

### 159. Write API retry logic.
```dart
Future<Response> getWithRetry(Dio dio, String path, {int retry = 3}) async {
  try {
    return await dio.get(path);
  } catch (_) {
    if (retry <= 1) rethrow;
    await Future.delayed(const Duration(seconds: 1));
    return getWithRetry(dio, path, retry: retry - 1);
  }
}
```

### 160. Write token refresh implementation.
```dart
dio.interceptors.add(
  InterceptorsWrapper(
    onError: (error, handler) async {
      if (error.response?.statusCode == 401) {
        final newToken = await refreshToken();
        error.requestOptions.headers['Authorization'] = 'Bearer $newToken';
        final response = await dio.fetch(error.requestOptions);
        return handler.resolve(response);
      }
      handler.next(error);
    },
  ),
);
```

### 161. Implement dark/light theme switching.
```dart
class ThemeProvider extends ChangeNotifier {
  ThemeMode themeMode = ThemeMode.light;

  void toggleTheme() {
    themeMode =
        themeMode == ThemeMode.light ? ThemeMode.dark : ThemeMode.light;
    notifyListeners();
  }
}
```

### 162. Create a reusable custom widget.
```dart
class AppButton extends StatelessWidget {
  final String text;
  final VoidCallback onPressed;

  const AppButton({
    super.key,
    required this.text,
    required this.onPressed,
  });

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,
      child: Text(text),
    );
  }
}
```

### 163. Implement pull-to-refresh functionality.
```dart
RefreshIndicator(
  onRefresh: () async {
    // refresh API
  },
  child: ListView.builder(
    itemCount: items.length,
    itemBuilder: (context, index) => Text(items[index]),
  ),
)
```

**Explain:** `RefreshIndicator` calls `onRefresh` when the user pulls the list down.

---

## SOLID Principles

### 164. What are SOLID principles?
SOLID is a set of five design principles used to write clean, maintainable, and scalable code.

- **S:** Single Responsibility Principle
- **O:** Open/Closed Principle
- **L:** Liskov Substitution Principle
- **I:** Interface Segregation Principle
- **D:** Dependency Inversion Principle

**Interview answer:** SOLID helps us reduce tight coupling, improve testability, and make code easier to change without breaking existing features.

### 165. Explain Single Responsibility Principle with Flutter example.
Single Responsibility Principle means one class should have only one main responsibility.

Bad example: one class handles UI, API call, validation, and local storage.

Good example:

- Widget handles UI.
- ViewModel/Bloc handles state.
- Repository handles data.
- API service handles network call.

**Memory line:** One class should have one reason to change.

### 166. Explain Open/Closed Principle.
Open/Closed Principle means code should be open for extension but closed for modification.

**Interview answer:** If a new feature comes, we should add new code instead of changing tested old code again and again.

Example: instead of writing many `if/else` conditions for payment types, create a common `PaymentMethod` interface and add new classes like `CardPayment`, `UpiPayment`, and `WalletPayment`.

### 167. Explain Liskov Substitution Principle.
Liskov Substitution Principle means a child class should be usable wherever the parent class is expected, without breaking behavior.

**Simple example:** If `Repository` has a `getData()` method, then `ApiRepository` and `MockRepository` should both work correctly wherever `Repository` is used.

**Memory line:** Child class should not break the promise of parent class.

### 168. Explain Interface Segregation Principle.
Interface Segregation Principle means do not force a class to implement methods it does not need.

Bad example: one big interface has `login`, `logout`, `uploadFile`, `makePayment`, and `sendNotification`.

Good example: split into smaller interfaces like `AuthService`, `FileUploadService`, `PaymentService`, and `NotificationService`.

### 169. Explain Dependency Inversion Principle.
Dependency Inversion Principle means high-level classes should depend on abstractions, not concrete classes.

**Flutter example:** A Bloc should depend on `AuthRepository`, not directly on `DioAuthApi`.

This makes testing easy because we can replace real API with fake/mock repository.

### 170. How do SOLID principles help in Clean Architecture?
SOLID supports Clean Architecture because it keeps code separated, testable, and loosely coupled.

- SRP separates UI, domain, and data responsibilities.
- OCP helps add new features without changing old code.
- DIP helps domain layer depend on repository interfaces, not API/database classes.

---

## OOP Concepts In Dart

### 171. What is OOP?
OOP means Object-Oriented Programming. It is a programming style where code is organized using classes and objects.

**Interview answer:** OOP helps us model real-world things, reuse code, hide internal details, and maintain large applications.

### 172. Difference between class and object.
A class is a blueprint. An object is a real instance created from that blueprint.

Example: `User` is a class. `User(name: 'Rahul')` is an object.

### 173. What is encapsulation?
Encapsulation means hiding internal data and exposing controlled access using methods or getters/setters.

```dart
class BankAccount {
  double _balance = 0;

  double get balance => _balance;

  void deposit(double amount) {
    if (amount > 0) _balance += amount;
  }
}
```

**Explain:** `_balance` is private, so outside code cannot change it directly.

### 174. What is inheritance?
Inheritance means one class can reuse properties and methods of another class using `extends`.

```dart
class Animal {
  void eat() => print('Eating');
}

class Dog extends Animal {
  void bark() => print('Barking');
}
```

### 175. What is abstraction?
Abstraction means showing only important details and hiding implementation details.

```dart
abstract class PaymentService {
  Future<void> pay(double amount);
}
```

The caller knows `pay()` exists, but does not need to know how payment is done internally.

### 176. What is polymorphism?
Polymorphism means one interface can have multiple implementations.

```dart
abstract class Shape {
  double area();
}

class Circle implements Shape {
  @override
  double area() => 3.14 * 5 * 5;
}

class Square implements Shape {
  @override
  double area() => 10 * 10;
}
```

**Explain:** Both classes use the same `area()` method name but provide different behavior.

### 177. Abstract class vs interface in Dart.
An abstract class can have method declarations and method implementations. In Dart, every class can be used as an interface with `implements`.

Use `extends` when you want to reuse base behavior. Use `implements` when you want to follow a contract and provide your own implementation.

### 178. Abstract class vs mixin.
An abstract class defines a base type or contract. A mixin is used to share reusable methods between classes without normal inheritance.

**Memory line:** Abstract class is for "is-a" relationship. Mixin is for shared ability.

### 179. Method overriding vs overloading.
**Overriding:** Child class provides its own implementation of a parent method.

**Overloading:** Same method name with different parameters. Dart does not support traditional method overloading. We use optional or named parameters instead.

### 180. Composition vs inheritance.
Inheritance means one class extends another class. Composition means one class uses another class inside it.

**Interview answer:** I prefer composition when possible because it reduces tight coupling and makes code more flexible.

---

## Dart Advanced Concepts

### 181. What is null safety in Dart?
Null safety means variables cannot contain `null` unless we explicitly allow it using `?`.

```dart
String name = 'Amit';
String? optionalName;
```

**Explain:** It helps avoid null pointer crashes at runtime.

### 182. What is the difference between nullable and non-nullable variables?
`String name` cannot be null. `String? name` can be null.

If a variable is nullable, we must handle null before using it.

### 183. What is late in Dart?
`late` means the variable will be initialized later before it is used.

```dart
late String token;
```

Use it carefully. If we use it before assigning a value, Dart throws an error.

### 184. Difference between dynamic, var, and Object.
`var` lets Dart infer the type and then the type is fixed.

`dynamic` disables type checking, so any method can be called at compile time.

`Object` can store any non-null value, but we need type checking/casting before using specific methods.

**Memory line:** Prefer specific type, use `var` for inference, avoid `dynamic` unless really needed.

### 185. What are generics in Dart?
Generics allow classes and methods to work with different types safely.

```dart
class ApiResponse<T> {
  final T data;
  ApiResponse(this.data);
}
```

**Explain:** `T` can be `User`, `Product`, `List<Order>`, etc.

### 186. What is a factory constructor?
A factory constructor controls object creation. It can return a new object, cached object, or object of a subclass.

```dart
class Logger {
  static final Logger _instance = Logger._internal();

  factory Logger() => _instance;

  Logger._internal();
}
```

### 187. What is a named constructor?
A named constructor gives multiple ways to create an object.

```dart
class User {
  final String name;

  User(this.name);

  User.guest() : name = 'Guest';
}
```

### 188. What is singleton pattern in Dart?
Singleton means only one object of a class is created and reused.

```dart
class AppConfig {
  AppConfig._();

  static final AppConfig instance = AppConfig._();
}
```

Use singleton for shared services like config, logger, or dependency container. Do not overuse it for business logic.

### 189. What are sealed, base, final, and interface classes in Dart?
These class modifiers control how classes can be extended or implemented.

- `sealed`: restricts subclassing to the same library.
- `final`: cannot be extended outside its library.
- `base`: child classes must also be base/final/sealed.
- `interface`: can be implemented but not extended outside its library.

**Interview answer:** These modifiers help make APIs safer and more predictable.

### 190. How do you use extension methods in real projects?
Extensions add helper methods to existing classes without changing them.

```dart
extension DateFormatExt on DateTime {
  String get shortDate => '$day/$month/$year';
}
```

I use extensions for formatting, validation, spacing helpers, and mapping UI values.

---

## Design Patterns

### 191. What is a design pattern?
A design pattern is a reusable solution for a common software design problem.

**Interview answer:** Design patterns help us write code that is easier to maintain, extend, and test.

### 192. Explain Singleton pattern.
Singleton ensures only one instance of a class exists.

Common examples: app configuration, logger, database helper, or service locator.

### 193. Explain Factory pattern.
Factory pattern creates objects without exposing object creation logic to the caller.

Example: based on API type, return `DevApiClient`, `StagingApiClient`, or `ProdApiClient`.

### 194. Explain Repository pattern.
Repository pattern hides data source details from the app.

The UI/domain calls repository methods. Repository decides whether data comes from API, cache, or database.

### 195. Explain Observer pattern.
Observer pattern means one object notifies many listeners when data changes.

Flutter examples: `ChangeNotifier`, streams, and Bloc state listeners.

### 196. Explain Strategy pattern.
Strategy pattern means we can switch algorithms without changing the caller code.

Example: different payment strategies like UPI, card, wallet, and cash on delivery.

### 197. Explain Adapter pattern.
Adapter pattern converts one format into another expected format.

Flutter example: converting API DTO model into domain entity:

```dart
UserEntity toEntity(UserDto dto) {
  return UserEntity(id: dto.userId, name: dto.fullName);
}
```

---

## Testing In Flutter

### 198. What are the types of testing in Flutter?
Flutter mainly has:

- **Unit test:** tests one function/class.
- **Widget test:** tests UI widget behavior.
- **Integration test:** tests full app flow.

### 199. Unit test vs widget test vs integration test.
Unit test is fastest and checks business logic. Widget test checks UI rendering and interactions. Integration test runs the app like a real user flow and is slower.

### 200. How do you test Provider/Bloc/Riverpod?
For Provider/Riverpod, I test state classes/providers directly and mock repositories. For Bloc, I use `bloc_test` to check that correct states are emitted after events.

### 201. What is mocking in testing?
Mocking means creating fake dependencies for tests.

Example: instead of calling real API, we use a fake repository that returns fixed data.

### 202. What are Mockito and Mocktail?
Mockito and Mocktail are Dart packages used to create mocks and verify method calls in tests.

**Interview answer:** I use them to test business logic without depending on real APIs, databases, or external services.

### 203. What is golden testing?
Golden testing compares the current UI screenshot with a saved expected screenshot.

It is useful for design-sensitive widgets, but it needs stable fonts, screen sizes, and test environment.

### 204. Why is testing important in Flutter projects?
Testing catches bugs early, protects old features during refactoring, improves confidence, and makes code easier to maintain in teams.

---

## Flutter Rendering & Layout

### 205. Widget tree vs Element tree vs RenderObject tree.
**Widget tree:** immutable configuration of UI.

**Element tree:** connects widgets with the actual mounted location in the tree.

**RenderObject tree:** handles layout, painting, and hit testing.

**Memory line:** Widget describes UI, Element manages lifecycle, RenderObject draws UI.

### 206. How does Flutter render UI?
Flutter builds widgets, creates elements, lays out render objects using constraints, paints layers, and sends the final scene to the engine for rendering.

**Simple answer:** Build decides what UI should be, layout decides size/position, paint draws it.

### 207. Explain Flutter constraint system.
In Flutter, parent gives constraints to child, child chooses its size within those constraints, and parent places the child.

**Memory line:** Constraints go down, sizes go up, parent sets position.

### 208. Why does layout overflow happen?
Overflow happens when a widget needs more space than its parent allows.

Common fixes:

- Use `Expanded` or `Flexible`.
- Use `SingleChildScrollView`.
- Reduce fixed width/height.
- Use responsive layout.

### 209. Difference between Expanded and Flexible.
`Expanded` forces child to take all available space. `Flexible` gives child space but allows it to be smaller.

### 210. What are IntrinsicHeight and IntrinsicWidth?
They measure child widgets based on their natural size. They can be expensive because Flutter may need extra layout passes.

**Interview answer:** I avoid intrinsic widgets in large lists or performance-sensitive screens.

### 211. What is CustomPaint?
`CustomPaint` lets us draw custom UI using Canvas and Paint.

Use it for charts, signatures, custom shapes, or complex visual effects.

### 212. What are Slivers in Flutter?
Slivers are scrollable parts used inside `CustomScrollView`.

Examples: `SliverAppBar`, `SliverList`, `SliverGrid`.

**Interview answer:** Slivers are useful for advanced scrolling effects like collapsing app bars and mixed scrollable layouts.

---

## Animations

### 213. Implicit vs explicit animations.
Implicit animations are simple and handled by Flutter, like `AnimatedContainer`.

Explicit animations give more control using `AnimationController`, `Tween`, and listeners.

### 214. What is AnimationController?
`AnimationController` controls animation duration, start, stop, reverse, and repeat.

It must be disposed in `dispose()` to avoid memory leaks.

### 215. What is Tween?
`Tween` defines start and end values for an animation.

Example: animate opacity from `0.0` to `1.0`, or size from `100` to `200`.

### 216. What is AnimatedBuilder?
`AnimatedBuilder` rebuilds only the animation-related part of UI when animation value changes.

It helps avoid rebuilding the full screen during animation.

### 217. What is Hero animation?
Hero animation creates a shared element transition between two screens.

Example: product image smoothly moves from product list screen to product detail screen.

### 218. How do you optimize animations?
Keep animated widget tree small, use `AnimatedBuilder`, avoid heavy work during animation, use `RepaintBoundary`, and profile with Flutter DevTools.

---

## Forms & Validation

### 219. How do forms work in Flutter?
Flutter uses `Form` widget with `TextFormField` and a `GlobalKey<FormState>` to validate and save form data.

### 220. How do you validate a form?
Call `formKey.currentState!.validate()`. Each field validator returns `null` if valid, or an error message if invalid.

### 221. Why should TextEditingController be disposed?
`TextEditingController` holds resources and listeners. If we do not dispose it, it can cause memory leaks.

### 222. What is FocusNode?
`FocusNode` controls keyboard focus for input fields.

Use it to move focus to next field, detect focus changes, or hide keyboard.

### 223. What is autovalidation?
Autovalidation means form fields validate automatically while the user types or interacts with the field.

Use `AutovalidateMode.onUserInteraction` for better user experience.

---

## Permissions & Native Features

### 224. How do you handle permissions in Flutter?
I use packages like `permission_handler`. I request permission at runtime, handle allowed/denied/permanently denied states, and show proper message to the user.

### 225. How do you handle camera, location, or storage permissions?
First add platform configuration in Android/iOS files. Then request runtime permission in Dart. If permission is denied, show explanation or open app settings.

### 226. What is MethodChannel real use case?
MethodChannel is used when Flutter package support is not available or we need custom native code.

Examples: custom SDK integration, device-specific APIs, native encryption, payment SDK, or hardware features.

### 227. What is background location?
Background location means tracking user location even when app is not in foreground.

It needs special permissions, user explanation, battery handling, and platform-specific configuration.

---

## CI/CD & Release

### 228. What is CI/CD?
CI/CD means Continuous Integration and Continuous Deployment/Delivery.

**Interview answer:** It automatically runs build, tests, code checks, and sometimes deploys app to testers or stores.

### 229. Which CI/CD tools are used for Flutter?
Common tools are GitHub Actions, Codemagic, Bitrise, Jenkins, and GitLab CI.

### 230. What is Fastlane?
Fastlane automates mobile release tasks like building APK/IPA, uploading to Play Store/TestFlight, managing screenshots, and handling signing steps.

### 231. What is Android signing?
Android signing means signing the APK/AAB with a keystore so Google Play and devices can verify the app owner.

### 232. What are iOS certificates and provisioning profiles?
iOS certificates prove developer identity. Provisioning profiles connect app ID, certificate, and allowed devices/capabilities.

### 233. How do you manage app versioning?
Flutter uses version in `pubspec.yaml`:

```yaml
version: 1.0.0+1
```

`1.0.0` is user-visible version. `+1` is build number.

### 234. What is the Play Store/App Store release process?
Build release app, sign it, test it, upload to Play Console/App Store Connect, fill release notes, submit for review, and monitor crashes after release.

---

## Accessibility, Localization & App Quality

### 235. What is accessibility in Flutter?
Accessibility means making the app usable for people with disabilities.

Use proper labels, readable text, enough contrast, large tap targets, and screen reader support using `Semantics`.

### 236. How do you support multiple languages in Flutter?
Use localization with `intl` or Flutter's localization tools. Store strings in ARB files and load text based on selected locale.

### 237. What is Remote Config?
Remote Config lets us change app values from Firebase without releasing a new app version.

Example: feature flags, minimum app version, offer text, or maintenance mode.

### 238. What are feature flags?
Feature flags allow enabling or disabling features without changing code.

They help with staged rollout, A/B testing, and quickly turning off risky features.

### 239. How do you reduce Flutter app size?
Use release build, remove unused assets/packages, compress images, use deferred loading, enable tree shaking, and analyze size using `flutter build apk --analyze-size`.

### 240. How do you optimize images in Flutter?
Use correct image size, compressed formats, caching, placeholders, and avoid loading huge images when small thumbnails are enough.

### 241. How do you track analytics events?
I track important user actions like login, signup, purchase, search, and screen views using tools like Firebase Analytics. I avoid sending sensitive personal data.

### 242. What is logging strategy in production?
In production, logs should be useful but not expose sensitive data. I use Crashlytics/Sentry breadcrumbs, API error logs, and important user flow events.

---

## More Integration Topics

### 243. How do WebSockets work in Flutter?
WebSocket keeps a continuous connection between app and server for real-time communication.

Use it for chat, live tracking, trading apps, and real-time notifications.

### 244. WebSocket vs REST API.
REST is request-response. WebSocket is continuous two-way communication.

Use REST for normal data fetching. Use WebSocket when server needs to push live updates.

### 245. How do you integrate payment gateway in Flutter?
Use official payment SDK/package, create payment order from backend, open payment UI, verify payment result from backend, and handle success/failure/cancel states.

### 246. How do you handle in-app purchases?
Use `in_app_purchase` package, configure products in Play Console/App Store Connect, purchase from app, verify receipt with backend, and unlock feature/subscription.

### 247. How do you integrate maps in Flutter?
Use `google_maps_flutter` or another map package. Configure API keys, permissions, markers, camera position, and location services.

### 248. How do you handle foreground, background, and terminated push notifications?
**Foreground:** app receives message directly and can show local notification.

**Background:** user taps notification and app opens target screen.

**Terminated:** app starts from notification payload, then navigates after initialization.

### 249. What is app obfuscation?
Obfuscation makes compiled Dart code harder to read after reverse engineering.

Use:

```bash
flutter build apk --obfuscate --split-debug-info=build/debug-info
```

Keep debug symbols safely because they are needed to read crash stack traces.

### 250. How do you handle GraphQL in Flutter?
Use a GraphQL client package, write queries/mutations, handle loading/error/data states, and cache results if needed.

**Interview answer:** GraphQL is useful when the client needs flexible data fields and wants to avoid over-fetching.
