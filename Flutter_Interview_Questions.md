# Flutter Developer Interview Questionnaire

This document contains professional, senior-level technical questions and answers designed for Flutter and Dart developer interviews.

---

## State Management

### 1. What state management techniques have you worked with in Flutter?
* **Answer:** I have extensive experience working with industry-standard state management solutions, including **Provider**, **Riverpod**, **Bloc (Business Logic Component)**, and **GetX**. These frameworks facilitate the decoupling of UI components from business logic, manage state lifecycles, and handle reactive rebuilds efficiently.

### 2. Why would you choose GetX over Provider or Bloc?
* **Answer:** GetX is chosen in projects where rapid prototyping and minimal boilerplate are prioritized. It provides state management, dependency injection, and route management in a single package without requiring `BuildContext`. However, for large-scale enterprise applications, Bloc or Riverpod is typically preferred to ensure structural discipline and testability.

### 3. What are the advantages and disadvantages of GetX?
* **Answer:** 
  * *Advantages:* High performance, no dependency on `BuildContext`, low boilerplate code, and unified utilities for routing and dependency management.
  * *Disadvantages:* Can lead to poorly structured, tightly coupled code if architectural guidelines are not strictly enforced. It bypasses Flutter's native widget tree propagation models, making debugging and unit testing more complex.

### 4. What is the difference between Provider, Riverpod, Bloc, and GetX?
* **Answer:**
  * *Provider:* Built on top of `InheritedWidget`. It propagates data down the widget tree using `BuildContext`.
  * *Riverpod:* A complete rewrite of Provider that operates outside the widget tree, eliminating provider lookup errors (e.g., `ProviderNotFoundException`), supporting compile-time safety, and allowing multiple instances of the same provider type.
  * *Bloc:* A strict, event-driven pattern based on Dart Streams. It enforces unidirectional data flow by receiving event inputs and producing state outputs, maximizing predictability and testability.
  * *GetX:* A micro-framework using reactive programming (Rx streams) and service locators. It bypasses `BuildContext` and the widget tree for state updates.

### 5. Why is Riverpod considered an improvement over Provider?
* **Answer:** Riverpod resolves key structural limitations of Provider. It does not rely on `BuildContext` to read states, making them accessible in business logic layers. It offers compile-time safety (no runtime `ProviderNotFoundException`), supports asynchronous state evaluation (`FutureProvider`, `StreamProvider`), and allows multiple independent instances of providers without complex keying.

### 6. What is the difference between reactive and non-reactive state management in GetX?
* **Answer:** 
  * *Reactive:* Utilizes observable streams (`Rx` types) and `Obx` or `GetX` widgets. The UI automatically rebuilds whenever the observable value changes, without manual triggers.
  * *Non-reactive:* Uses standard variables and calls `update()` within a `GetxController` subclass, coupled with a `GetBuilder` widget. This requires manual state-dispatching calls, similar to `ChangeNotifier`.

### 7. Explain Rx variables in GetX.
* **Answer:** Rx variables (e.g., `RxInt`, `RxString`, or appending `.obs` to primitives) are wrappers that turn variables into reactive streams (observables). Under the hood, they use Dart Streams to broadcast value updates to listening widgets (`Obx`), triggering targeted UI rebuilds.

### 8. What is the purpose of notifyListeners() in Provider?
* **Answer:** `notifyListeners()` is a method of `ChangeNotifier`. When called, it dispatches a notification to all active `ListenableProvider` listeners down the widget tree, indicating that the model's state has mutated and prompting dependent widgets to rebuild.

### 9. When would you use Bloc instead of GetX?
* **Answer:** Bloc is preferred for enterprise-grade, multi-developer projects that require a strict architecture, absolute predictability, and extensive test coverage. It enforces unidirectional data flow and separates user interactions (events) from UI modifications (states), which makes debugging complex state flows straightforward.

### 10. How does state persist after app refresh or app restart?
* **Answer:** State persistence is achieved by serializing state models (typically to JSON) and writing them to local persistent storage (e.g., `shared_preferences` for key-value configurations, `Hive` for fast NoSQL databases, or `sqflite` for relational databases) upon mutation, then deserializing and loading this data during app initialization.

---

## Flutter Architecture

### 11. Which architecture pattern have you used in Flutter projects?
* **Answer:** I primarily implement **Clean Architecture** combined with **MVVM (Model-View-ViewModel)** or feature-based modular patterns, ensuring clear separation of concerns, testability, and decoupled domain logic.

### 12. Explain Clean Architecture in Flutter.
* **Answer:** Clean Architecture separates the codebase into decoupled layers with strict dependency rules pointing inward: **Presentation**, **Domain**, and **Data**. This division isolates core business logic from framework-specific dependencies (such as UI widgets, databases, and network clients).

### 13. What are the responsibilities of Presentation, Domain, and Data layers?
* **Answer:**
  * *Presentation:* Handles UI widgets, input collection, and state management controllers (e.g., Blocs, ViewModels) that listen to domain use cases.
  * *Domain:* Represents the core business rules of the application. It contains entity definitions, use cases, and repository interfaces. This layer is written in pure Dart and has no dependencies on external frameworks or database packages.
  * *Data:* Manages database transactions, network API client communications, model serialization, and implements the repository interfaces defined in the Domain layer.

### 14. What are Use Cases in Clean Architecture?
* **Answer:** A Use Case (or Interactor) encapsulates a single, discrete business logic operation (e.g., `ExecuteUserLogin`, `GetCartTotal`). It defines the boundaries of user interaction and coordinates domain entities, serving as the sole entry point for the presentation layer to interact with domain rules.

### 15. What is the role of Repository classes?
* **Answer:** A Repository acts as an abstraction barrier between the business logic (Domain) and the data sources (Data). It defines interfaces in the Domain layer for data access. The Presentation layer calls these interfaces, while the Data layer implements them, orchestrating caching strategies (e.g., switching between local SQL storage and remote REST endpoints).

### 16. How do layers communicate with each other?
* **Answer:** Layers communicate using unidirectional interfaces. The Presentation layer invokes Domain Use Cases. The Use Case accesses Data repositories via interfaces defined in the Domain layer (following the Dependency Inversion Principle). Data returns back to the presentation layer in the form of domain entities.

### 17. How would you structure a Flutter project with Login, Dashboard, and Profile?
* **Answer:** I utilize a **feature-first** modular structure:
  ```text
  lib/
  ├── features/
  │   ├── login/
  │   │   ├── data/
  │   │   ├── domain/
  │   │   └── presentation/
  │   ├── dashboard/
  │   └── profile/
  └── core/ (shared utilities, themes, and networking clients)
  ```

### 18. Where would you place Providers, Bloc classes, APIs, Models, and Repositories?
* **Answer:**
  * *Blocs/Providers (ViewModels):* Located in the `presentation` layer of their respective features.
  * *APIs (Data Sources) & Models:* Located in the `data` layer.
  * *Repositories:* Interfaces are declared in the `domain` layer; concrete implementations are placed in the `data` layer.

### 19. What are the benefits of feature-based folder structure?
* **Answer:** It enhances scalability and modularity. Developers can modify, refactor, or delete a feature within its self-contained directory without impacting other parts of the application. It also simplifies code navigation and testing in large team environments.

### 20. How do you maintain scalability in large Flutter applications?
* **Answer:** Scalability is maintained by enforcing strict architectural boundaries (e.g., Clean Architecture), using dependency injection (e.g., `get_it`), keeping widgets atomic, writing unit/widget/integration tests, and using modular package structures (monorepos) to isolate features.

---

## Navigation & Session Handling

### 21. How do you manage navigation in Flutter?
* **Answer:** I manage navigation using **GoRouter** (the official declarative routing system) or Flutter's imperative `Navigator` API. Declarative routing is preferred as it binds the navigation stack directly to app state and handles deep linking seamlessly.

### 22. What is the difference between Navigator and GetX navigation?
* **Answer:** 
  * *Navigator:* Requires a valid `BuildContext` to look up the routing state in the widget tree.
  * *GetX:* Uses a global key registered under a custom `GetMaterialApp` context, allowing developers to invoke imperative transitions (e.g., `Get.to()`) from clean business logic files without passing a `BuildContext`.

### 23. How do you restore navigation state after refresh?
* **Answer:** When compiling for Flutter Web, we use declarative routers like GoRouter to map the browser's URL paths directly to specific widget trees. The router parses URL state parameters on application reload and reconstructs the navigation stack.

### 24. How do you persist session data in Flutter?
* **Answer:** Session tokens (e.g., JWT access and refresh tokens) are persisted securely in device hardware-backed storage (Keychain on iOS, Keystore on Android) using packages like `flutter_secure_storage`. Non-sensitive state is persisted via key-value stores like `shared_preferences`.

### 25. What happens when a user refreshes a Flutter web application?
* **Answer:** The app's Dart virtual machine restarts, wiping all in-memory states. To prevent data loss, the application must read the browser's URL to determine the active route and load user sessions from persistent local storage (e.g., `localStorage`).

### 26. How would you save the last visited screen?
* **Answer:** I would implement a navigation observer that captures route changes and writes the target route name to `shared_preferences` on transition. During application startup, this key is queried, and the router directs the user to the saved path.

---

## Async Programming

### 27. What is the difference between FutureBuilder and StreamBuilder?
* **Answer:**
  * *FutureBuilder:* Listens to a `Future` and triggers a single rebuild when the asynchronous operation completes (resolved or rejected).
  * *StreamBuilder:* Listens to a `Stream` and triggers multiple rebuilds sequentially as new data payloads are emitted over time.

### 28. What is the difference between BlocBuilder and FutureBuilder?
* **Answer:**
  * *FutureBuilder:* Designed for simple, one-time asynchronous lifecycle tasks (e.g., reading a file or calling an API) directly inside the widget tree.
  * *BlocBuilder:* Designed for complex, state-driven UIs, rebuilds in response to state transitions generated by a Bloc class, and separates the trigger logic from the presentation layer.

### 29. When should you use FutureBuilder?
* **Answer:** When handling a single asynchronous task directly within the UI, such as fetching user settings, loading local assets, or resolving a one-time API request where local cache lookup is sufficient.

### 30. When should you use StreamBuilder?
* **Answer:** When displaying reactive, real-time data streams in the UI, such as chat message delivery feeds, WebSockets connections, location tracking updates, or listening to database query streams (e.g., Firestore snapshots).

### 31. How do you handle API loading, success, and error states?
* **Answer:** I represent the network status in the state layer using a wrapper object (or discrete states in Bloc, e.g., `LoadingState`, `SuccessState`, `ErrorState`). The UI evaluates this state and displays appropriate layouts (e.g., circular progress indicator for loading, data layout for success, and error messages/re-attempt controls for errors).

---

## Flutter Web

### 32. Have you worked on Flutter Web projects?
* **Answer:** Yes. Flutter Web allows developers to compile existing Dart codebases into HTML/CSS/JS and WebAssembly, facilitating multi-platform delivery across browsers.

### 33. What are the challenges in Flutter Web?
* **Answer:** Key challenges include initial bundle size (leading to longer load times), compatibility limitations of mobile-only packages, search engine optimization (SEO) optimization, and managing layout layouts for wide computer viewports.

### 34. How do you make Flutter Web responsive?
* **Answer:** By using layout tools like `LayoutBuilder`, `MediaQuery`, `Flex`/`Row`/`Column`, and helper packages such as `responsive_builder`. I define viewport breakpoints to adapt layouts dynamically between mobile, tablet, and desktop screens.

### 35. What is the difference between MediaQuery and LayoutBuilder?
* **Answer:**
  * *MediaQuery:* Returns the global dimensions, orientation, and padding of the entire device screen.
  * *LayoutBuilder:* Passes the `BoxConstraints` of the immediate parent widget, letting you adapt layouts based on the layout space allocated to that specific widget.

### 36. How do you optimize Flutter Web performance?
* **Answer:** I optimize performance by using deferred/lazy loading of routes, compressing assets, applying tree-shaking, caching resources with Service Workers, and selecting the optimal web renderer (`html` for fast initial loads, `canvaskit` for graphic-intensive animations).

### 37. How do you handle browser refresh in Flutter Web?
* **Answer:** By using declarative routing (e.g., GoRouter) to maintain URL mapping and saving app state (such as auth tokens or user configurations) to browser storage (`localStorage` or `sessionStorage`) so it can be restored on reload.

---

## File Upload & API Integration

### 38. How do you upload files in Flutter?
* **Answer:** By selecting a file using `file_picker` and constructing a HTTP POST request containing `multipart/form-data`. Using libraries like `dio`, the file is wrapped in a `MultipartFile` object and transmitted with upload progress callbacks.

### 39. How do you upload files in Flutter Web?
* **Answer:** On Flutter Web, direct access to the local filesystem path is restricted for security. Instead, we read the selected file as a byte array (`Uint8List`) from memory, wrap it inside a `MultipartFile.fromBytes` container, and upload it via HTTP.

### 40. What is multipart/form-data?
* **Answer:** It is an HTTP request standard that allows files (binary data) to be sent along with text fields in a single request body, separating the payloads using boundary strings.

### 41. What packages have you used for file upload?
* **Answer:** I typically use `file_picker` or `image_picker` to handle user selection, and `dio` or `http` to execute the file upload network requests.

### 42. How do you handle platform-specific implementations in Flutter?
* **Answer:** I implement platform-specific tasks using conditional imports, abstracting class implementations per platform, or using `MethodChannel` / platform channels to run native Java/Kotlin (Android) or Objective-C/Swift (iOS) code.

### 43. What is kIsWeb in Flutter?
* **Answer:** It is a compile-time boolean constant in Flutter's `foundation` library. It evaluates to `true` if the application compiles for web browsers, allowing conditional platform execution.

---

## Dependency & Deployment

### 44. What happens when a new package dependency is added?
* **Answer:** When added to `pubspec.yaml`, the developer executes `flutter pub get`. The package manager retrieves the package from pub.dev, resolves dependency versions, and updates `pubspec.lock` to lock the resolved dependency tree for consistency across environments.

### 45. What issues can occur after deploying to production?
* **Answer:** Common post-deployment issues include crashes on specific OS versions, unexpected layout overflows due to differing screen sizes, API integration failures (e.g., SSL validation errors), and latency on low-end devices.

### 46. How do you manage package versions in Flutter?
* **Answer:** I use semantic versioning caret syntax (e.g., `^1.2.0`) in `pubspec.yaml` to allow backwards-compatible updates, while relying on `pubspec.lock` to commit and lock build environments across development teams.

### 47. What is dependency conflict?
* **Answer:** It occurs when two dependencies in your project require different, incompatible versions of a shared third-party library. This is resolved by upgrading packages, applying dependency overrides in `pubspec.yaml`, or requesting authors to update dependencies.

### 48. How do you debug production issues?
* **Answer:** I integrate crash reporting SDKs like **Firebase Crashlytics** or **Sentry** to capture stack traces, console breadcrumbs, and device metadata from production environments.

---

## Flutter Fundamentals

### 49. What is the widget lifecycle in Flutter?
* **Answer:** It represents the state transitions of a `StatefulWidget`:
  1. `createState()`: Creates the mutable state object.
  2. `initState()`: Performs one-time setup initialization.
  3. `didChangeDependencies()`: Called when dependency objects (like `InheritedWidget`) change.
  4. `build()`: Renders the widget structure on screen.
  5. `didUpdateWidget()`: Invoked when the parent widget changes properties.
  6. `deactivate()`: Called when the state object is temporarily removed from the tree.
  7. `dispose()`: Discards resources, closes streams, and cleans up memory permanently.

### 50. Difference between StatelessWidget and StatefulWidget?
* **Answer:**
  * *StatelessWidget:* Immutable. Its properties cannot change at runtime once built. Rebuilds only when the parent configuration changes.
  * *StatefulWidget:* Mutable. Maintains a separate `State` object that persists across rebuilds, allowing dynamic UI modifications via `setState()`.

### 51. What are Keys in Flutter?
* **Answer:** Keys are identifiers for widgets. They help Flutter's element tree map widgets to their underlying element state when widgets are moved, modified, or reordered within the tree.

### 52. What is BuildContext?
* **Answer:** `BuildContext` is an interface that represents a handle to the location of a widget within the widget tree. It is used to traverse the tree to find inherited widgets, locate theme data, or search for ancestor states.

### 53. What causes unnecessary widget rebuilds?
* **Answer:** Triggering `setState()` on root widgets, missing `const` constructors on static layouts, calling providers high up in the widget tree, or failing to use selectors/filters when listening to state updates.

### 54. How do you optimize Flutter app performance?
* **Answer:**
  * Annotate widgets with `const` where possible to skip rebuild steps.
  * Implement lazy-loading lists (`ListView.builder`).
  * Run CPU-heavy tasks on separate Dart Isolates.
  * Cache images locally using `cached_network_image`.
  * Minimize the scope of rebuilding widgets (e.g., using `Selector` in Provider or `RepaintBoundary` for animations).

### 55. What are mixins in Dart?
* **Answer:** Mixins allow classes to reuse code from other class structures without requiring subclassing (multiple inheritance). They are applied using the `with` keyword.

### 56. What are isolates in Flutter?
* **Answer:** Isolates are Dart's implementation of execution threads. Unlike standard threads, isolates do not share memory; they run in their own isolated memory heap and communicate exclusively using asynchronous message passing (ports).

---

## Practical & Scenario-Based Questions

### 57. How would you design a scalable e-commerce Flutter app?
* **Answer:** I would apply Clean Architecture, split the project into isolated feature modules (Auth, Product, Checkout), manage state using Bloc or Riverpod, use `get_it` for dependency injection, write structured API clients with Dio, and use local database adapters for caching (like Hive).

### 58. Suppose the API fails — how would your UI behave?
* **Answer:** The UI should fail gracefully. Instead of displaying a broken screen, it should display an error state, provide a descriptive message (e.g., "Network Timeout"), and present a retry button to re-trigger the data fetch.

### 59. How would you implement offline support?
* **Answer:** By caching fetched API responses in local storage (e.g., SQLite or Hive database). When the app requests data, the repository layer returns the cached version first while requesting fresh data from the API in the background.

### 60. How would you handle app state after internet reconnect?
* **Answer:** I would use the `connectivity_plus` package to monitor network changes. Upon connection recovery, the app syncs pending offline actions (stored in a local database queue) and fetches updated data.

### 61. How would you secure sensitive data in Flutter?
* **Answer:** I would encrypt critical local databases (e.g., using encrypted Hive boxes or SQLCipher) and store authentication tokens and API secrets in secure OS-level storage (Keychain for iOS, Keystore for Android) via `flutter_secure_storage`.

### 62. How would you handle deep linking?
* **Answer:** By configuring URL schemes and universal/app links on Android (`intent-filters`) and iOS (`Associated Domains`). I use routers like GoRouter to parse incoming links and navigate the user to target screens.

### 63. Explain a challenging bug you solved recently.
* **Answer:** I resolved a memory leak caused by un-disposed animation controllers and Stream subscriptions, which crashed the app on low-end devices. I fixed it by auditing the `dispose()` lifecycles and utilizing automated leak tracking tools.

### 64. Explain a performance issue you faced in Flutter and how you fixed it.
* **Answer:** In a long list of complex items, scrolling was laggy due to excessive widget rebuilds. I optimized it by adding `const` constructors, wrapping items in `RepaintBoundary` to isolate graphics rendering, and using `ListView.builder` for lazy widget instantiation.

### 65. If your app crashes only in production, how would you debug it?
* **Answer:** I would inspect Sentry or Firebase Crashlytics reports to review the production stack trace. I would also run a release build locally on a physical test device to inspect output logs.

---

## Team & Project Experience

### 66. How do you collaborate with backend teams?
* **Answer:** By standardizing API contracts using Swagger/OpenAPI documentation. We align on endpoints, JSON payloads, HTTP codes, and authentication procedures before implementation begins.

### 67. How do you manage code reviews?
* **Answer:** I use Git Pull Requests. I review code for architectural consistency, performance issues, test coverage, and security vulnerabilities, leaving constructive comments before merging.

### 68. Have you worked in Agile/Scrum?
* **Answer:** Yes, I work in Agile teams with 2-week sprints, participating in Sprint Planning, daily standups, backlog refinement, and Sprint Retrospectives.

### 69. How do you handle merge conflicts?
* **Answer:** I analyze the conflicting code sections, locate the changes, coordinate with the respective developer if necessary to verify requirements, apply the correct merge fix, and run unit tests to verify stability.

### 70. What role do you usually play in your project team?
* **Answer:** I typically act as a Senior Flutter Engineer, driving architectural decisions, writing clean code, maintaining CI/CD pipelines, and mentoring junior developers.

---

## API & Networking

### 71. What is the difference between REST API and GraphQL?
* **Answer:**
  * *REST:* Relies on multiple fixed-endpoint URIs that return predefined JSON data structures, which can lead to over-fetching or under-fetching.
  * *GraphQL:* Uses a single endpoint where clients submit declarative queries to request exactly the fields they need, reducing payload size.

### 72. How do you handle multiple API calls simultaneously in Flutter?
* **Answer:** I use `Future.wait()`, which executes multiple asynchronous tasks concurrently and completes once all individual futures are resolved.

### 73. What is Dio in Flutter and why is it preferred over HTTP package?
* **Answer:** Dio is a powerful HTTP client package for Dart. It supports advanced networking features out of the box, such as Interceptors, global configuration, request cancellation, file upload/download progress, request timeout configurations, and automatic retries.

### 74. What are interceptors in Dio?
* **Answer:** Interceptors allow developers to intercept and modify HTTP requests, responses, or errors before they are processed by the caller.

### 75. Explain onRequest, onResponse, and onError in Dio interceptors.
* **Answer:**
  * *onRequest:* Executed before a request is sent. Used to add auth tokens or configure global headers.
  * *onResponse:* Executed when a response is received. Used to log payloads or transform data globally.
  * *onError:* Executed when a request fails. Used to intercept error statuses (like 401 Unauthorized) to attempt token refresh or global error redirection.

### 76. How do you add authentication tokens using interceptors?
* **Answer:** Inside `onRequest`, we retrieve the active access token from secure storage and append it to the `Authorization` header of the `RequestOptions` object before forwarding it.

### 77. How do you refresh expired JWT tokens automatically?
* **Answer:** Inside `onError`, if the error code is `401 Unauthorized`, we lock the Dio queue, request a new JWT token using a refresh token endpoint, update secure storage, apply the new token to the failed request, and re-execute it.

### 78. What is global error handling in Flutter networking?
* **Answer:** A centralized exception handling pattern that intercepts all network anomalies (timeouts, validation failures, authentication drops) and resolves them globally, showing standard user dialogs or toast notifications.

### 79. How do you handle API timeout scenarios?
* **Answer:** I configure `connectTimeout`, `receiveTimeout`, and `sendTimeout` parameters on the Dio instance. If a timeout threshold is exceeded, a `DioException` of type `timeout` is thrown, which the app catches to show a timeout message.

### 80. How do you implement retry mechanisms for failed APIs?
* **Answer:** I write custom retry logic in a Dio interceptor or use helper packages like `dio_smart_retry`. If a request fails due to temporary network issues, the client re-attempts the request up to a set limit.

### 81. What is middleware? How is it different from interceptors?
* **Answer:**
  * *Middleware:* Operates at the application route level, intercepting user navigation requests to check permissions before rendering a target screen.
  * *Interceptors:* Operate at the network transport layer, intercepting outbound HTTP requests and inbound HTTP responses.

### 82. Where are middleware commonly used?
* **Answer:** Commonly used in navigation systems (like GoRouter) to redirect unauthenticated users away from private routes (e.g., `/dashboard`) back to the `/login` screen.

### 83. Can interceptors modify request headers and responses?
* **Answer:** Yes, they have access to request options and response objects, allowing headers to be added or response formats to be standardized.

### 84. How do you log API requests and responses in Flutter?
* **Answer:** By configuring a custom logger or using open-source packages like `PrettyDioLogger` inside the Dio interceptor list to print formatted networking logs to the console during development.

### 85. What is request cancellation in Dio?
* **Answer:** It is the ability to abort an active HTTP request using a `CancelToken`. If a user navigates away from a screen, cancelling pending requests saves device bandwidth and resource overhead.

### 86. How do you upload files/images using Dio?
* **Answer:** By wrapping the file binary inside a `MultipartFile` and attaching it to a `FormData` payload, which is then sent via a POST or PUT request.

### 87. How do you download large files efficiently in Flutter?
* **Answer:** Using Dio's `download()` API, saving the file directly to the device storage path, and listening to the `onReceiveProgress` callback to update download progress indicators in the UI.

### 88. How do you secure API calls in Flutter applications?
* **Answer:** By enforcing HTTPS connection protocols, configuring SSL Pinning to prevent man-in-the-middle (MITM) attacks, and storing sensitive API keys in native builds rather than hardcoding them in Dart code.

---

## Additional State Management

### 89. Which state management approaches have you used in Flutter?
* **Answer:** I have built production applications using **Provider**, **Riverpod**, **Bloc**, and **GetX**, matching the state management tool to the complexity and scale of the project.

### 90. Difference between Provider, Bloc, Riverpod, and GetX?
* **Answer:**
  * *Provider:* InheritedWidget wrapper using `BuildContext` lookup.
  * *Riverpod:* Compile-safe, independent of `BuildContext`, compile-time checked state container.
  * *Bloc:* Stream-based, unidirectional event-to-state state machine.
  * *GetX:* Context-free reactive controller wrapper using service locators.

### 91. What is the difference between setState and Provider?
* **Answer:**
  * *setState:* Rebuilds the current StatefulWidget and its descendants. It is only suitable for simple local widget state.
  * *Provider:* Decoupled state container. It supports global state sharing and fine-grained rebuilds of specific subscriber widgets (e.g. using `Consumer`).

### 92. When would you choose Bloc over Provider?
* **Answer:** When building large applications with complex business rules, multiple developers, and a need for strict unidirectional data flow, predictability, and unit testing.

### 93. Explain reactive programming in Flutter.
* **Answer:** It is a programming paradigm focused on data streams and change propagation. The UI is built as a function of the active state ($UI = f(State)$), automatically updating itself when changes flow through reactive streams.

### 94. What are streams in Dart?
* **Answer:** Streams are asynchronous sequences of data events. They emit values, errors, or close notifications over time. Listeners subscribe to streams to process these events as they occur.

### 95. Difference between StreamBuilder and FutureBuilder?
* **Answer:**
  * *FutureBuilder:* Renders UI based on a one-off asynchronous calculation that completes once.
  * *StreamBuilder:* Listens to a continuous stream of events, rebuilding the UI whenever new data is emitted.

---

## Flutter Core Concepts

### 96. Difference between StatelessWidget and StatefulWidget?
* **Answer:** StatelessWidget is immutable and lacks a persistent state lifecycle. StatefulWidget maintains a separate mutable State object that persists across widget rebuilds.

### 97. What is widget lifecycle in Flutter?
* **Answer:** The stages a StatefulWidget goes through: `createState` $\to$ `initState` $\to$ `didChangeDependencies` $\to$ `build` $\to$ `didUpdateWidget` $\to$ `deactivate` $\to$ `dispose`.

### 98. Difference between initState(), build(), and dispose()?
* **Answer:**
  * *initState():* One-time setup, called once when the state object is created.
  * *build():* Runs whenever the UI needs to be rendered or updated.
  * *dispose():* Clean-up method, called when the widget is permanently removed from the tree.

### 99. What are keys in Flutter?
* **Answer:** Keys preserve widget state when widgets are moved around in the widget tree, ensuring that element configurations match their corresponding widgets.

### 100. Difference between GlobalKey and ValueKey?
* **Answer:**
  * *ValueKey:* Used locally within a parent widget (e.g., lists) to distinguish widgets with matching types based on value comparison.
  * *GlobalKey:* Globally unique across the entire application. It allows access to a widget's state from anywhere in the app, but has a higher performance overhead.

### 101. What is BuildContext?
* **Answer:** An interface representing the location of a widget in the widget tree, used to locate inherited resources (like themes or providers).

### 102. What causes widget rebuilds?
* **Answer:** Rebuilds are triggered by calls to `setState()`, parent widget configuration changes, or modifications to inherited widgets (like Providers) that the widget is subscribed to.

### 103. How do you optimize Flutter app performance?
* **Answer:** Add `const` constructors to static widgets, use `ListView.builder` for lists, implement `RepaintBoundary` around animations, avoid doing heavy tasks inside `build()`, and run CPU-heavy operations on separate Isolates.

### 104. What is const constructor optimization?
* **Answer:** It creates a compile-time constant widget. Flutter registers this instance in memory, allowing it to skip rebuilding this widget when its parent rebuilds.

### 105. What are mixins in Dart?
* **Answer:** Mixins allow classes to reuse methods and properties from other classes without using inheritance.

### 106. Difference between final and const?
* **Answer:**
  * *const:* Evaluated at compile-time. Value must be known during compilation.
  * *final:* Evaluated at runtime. Can only be set once.

### 107. What are extensions in Dart?
* **Answer:** They allow you to add methods and properties to existing classes (e.g., adding formatting methods to `String` or `DateTime`) without modifying the original class source code.

---

## Isolates & Performance

### 108. What are isolates in Flutter?
* **Answer:** Dart's execution model. Unlike traditional threads, isolates run in separate memory heaps, communicating only through message passing via ports.

### 109. Why do we use isolates?
* **Answer:** To run CPU-heavy tasks (like data parsing, image processing, or cryptography) without blocking the main event loop, maintaining a smooth 60/120 FPS UI.

### 110. Difference between async/await and isolates?
* **Answer:**
  * *async/await:* Asynchronous tasks are scheduled on the single main thread event loop.
  * *Isolates:* Running tasks in parallel on separate CPU cores with independent memory heaps.

### 111. What tasks should be moved to isolates?
* **Answer:** Heavy JSON parsing, image processing, file compression/decompression, and sorting large datasets.

### 112. Explain compute() in Flutter.
* **Answer:** A helper function that spawns an isolate, runs a callback function with arguments, returns the result asynchronously, and automatically shuts down the isolate.

### 113. How do isolates communicate with each other?
* **Answer:** Using `SendPort` and `ReceivePort` objects. Isolates send serialized messages through these ports since they do not share memory.

### 114. What happens if heavy processing runs on the main thread?
* **Answer:** It blocks the main event loop, causing dropped frames (jank) and temporary UI freezing.

### 115. Difference between concurrency and parallelism?
* **Answer:**
  * *Concurrency:* One thread switches tasks back and forth rapidly, making progress on multiple tasks.
  * *Parallelism:* Multiple CPU cores running separate tasks simultaneously.

---

## Local Storage & Caching

### 116. Difference between SharedPreferences, Hive, and SQLite?
* **Answer:**
  * *SharedPreferences:* Simple key-value store for simple settings (e.g. preferences).
  * *Hive:* Fast, lightweight NoSQL key-value database written in pure Dart.
  * *SQLite:* Relational database for structured, complex datasets with relationships and SQL query support.

### 117. How do you cache API responses?
* **Answer:** Save the JSON responses to a local database (like Hive or SQLite) with a timestamp. When requesting data, the app returns the cached data first, updating it once the new API call completes.

### 118. What is offline-first architecture?
* **Answer:** Designing the app to store all data locally first. The UI reads only from local storage, and a sync layer updates this local database with the server in the background.

### 119. How do you store sensitive data securely in Flutter?
* **Answer:** By encrypting the local database using cryptographic keys and storing sensitive tokens in secure OS-level storage (Keychain for iOS, Keystore for Android).

### 120. What is Flutter Secure Storage?
* **Answer:** A package that wraps iOS Keychain and Android Keystore APIs, providing secure storage for keys and tokens.

---

## Architecture

### 121. Which architecture patterns have you used?
* **Answer:** I have implemented Clean Architecture, MVVM, and MVC in production environments.

### 122. MVC
* **Answer:** *Model-View-Controller:* Model manages data, View displays the UI, and Controller updates the Model and updates the View in response to inputs.

### 123. MVVM
* **Answer:** *Model-View-ViewModel:* Model represents the data layer, View displays the UI, and ViewModel acts as a state coordinator, preparing data from the Model for the View to display.

### 124. Clean Architecture
* **Answer:** A design pattern that divides the app into Data, Domain, and Presentation layers, enforcing strict dependencies pointing inward to protect business rules.

### 125. Explain Clean Architecture in Flutter.
* **Answer:** The presentation layer holds UI widgets, the domain layer holds pure business logic (entities, use cases), and the data layer implements repositories to fetch data from APIs and databases.

### 126. What is repository pattern?
* **Answer:** Decouples the domain layer from data sources, providing a single data access interface.

### 127. What is dependency injection?
* **Answer:** A design pattern where class dependencies are passed (injected) to a class rather than created by the class itself, improving testability and code reuse.

### 128. Have you used GetIt or Injectable?
* **Answer:** Yes, I use `get_it` as a service locator to register dependencies, and `injectable` to generate the registration code using annotations.

### 129. Difference between tightly coupled and loosely coupled code?
* **Answer:** Tightly coupled code has classes that directly depend on other concrete implementations, making modifications difficult. Loosely coupled code uses abstract interfaces, allowing implementations to be swapped out without affecting other classes.

---

## Firebase

### 130. Have you integrated Firebase in Flutter?
* **Answer:** Yes, I have integrated Firebase services such as Firestore, Authentication, Cloud Messaging, Crashlytics, and Analytics.

### 131. Difference between Firebase Authentication and Firestore?
* **Answer:** Firebase Authentication handles user registration, login, and access tokens. Firestore is a NoSQL document database used to store application data.

### 132. How do push notifications work in Flutter?
* **Answer:** The app registers the device token with FCM. The backend sends a message payload to FCM, which routes the notification to the target device. The OS displays the notification or passes the payload to the app.

### 133. What is Firebase Cloud Messaging (FCM)?
* **Answer:** A cloud service by Google that allows you to send notifications and message payloads to Android, iOS, and Web clients.

### 134. How do you handle background notifications?
* **Answer:** By registering a top-level or static background message handler function. The OS wakes up a background isolate to process the message payload even if the app is closed.

---

## Advanced Flutter

### 135. What are platform channels?
* **Answer:** A message-passing bridge that allows Dart code to communicate with native Kotlin/Java (Android) or Swift/Objective-C (iOS) APIs.

### 136. How does Flutter communicate with native Android/iOS code?
* **Answer:** By sending message payloads via `MethodChannel` (for one-off method calls) or `EventChannel` (for continuous data streams).

### 137. Difference between Future and Stream?
* **Answer:** A Future represents a single asynchronous value (resolved or rejected). A Stream represents a sequence of asynchronous values emitted over time.

### 138. What is code splitting?
* **Answer:** Compiling the application into separate, smaller deferred bundles so that only necessary resources are loaded on startup, reducing initial download size.

### 139. What is tree shaking in Flutter?
* **Answer:** A compilation optimization step that removes unused dead code from the final binary, reducing app package size.

### 140. What are flavors/environments in Flutter?
* **Answer:** Creating distinct build configurations (e.g. dev, staging, prod) to point the app to different API servers and bundle IDs.

### 141. How do you manage dev, staging, and production builds?
* **Answer:** By defining compile-time environment configurations (using `--dart-define` or product flavors) to swap API keys, database settings, and target backend URLs during compilation.

### 142. What are deep links?
* **Answer:** URLs that redirect mobile devices to open a specific screen in the application instead of rendering in a browser.

### 143. What is app lifecycle in Flutter?
* **Answer:** The execution states of the application: `resumed` (foreground), `inactive` (transitioning), `paused` (background), or `detached` (terminated).

---

## Practical Scenarios

### 144. How would you design a dashboard with 5 simultaneous API calls?
* **Answer:** Execute all 5 calls concurrently using `Future.wait()`, show a loading layout, and render the complete dashboard layout once all futures resolve.

### 145. How would you avoid duplicate API calls during screen rebuild?
* **Answer:** Store the Future in a state variable inside `initState()` of a `StatefulWidget`, rather than creating the Future inside the `build()` method.

### 146. How would you optimize a slow Flutter app?
* **Answer:** Profile using DevTools, add `const` constructors, use `ListView.builder` for lists, implement `RepaintBoundary` for animations, and offload CPU-intensive tasks to Isolates.

### 147. How would you handle session expiration globally?
* **Answer:** Implement a network interceptor that checks for `401 Unauthorized` responses. If caught, it triggers a logout event, clears session data, and redirects the user to the login route.

### 148. How would you implement pagination?
* **Answer:** Attach a listener to a `ScrollController`. When the scroll position approaches the maximum scroll extent, trigger an API request to fetch the next page of items.

### 149. How would you implement search with debounce?
* **Answer:** Use a timer in the text field listener. Reset the timer on each keystroke, executing the API search request only after the user stops typing for a set delay (e.g. 500ms).

### 150. How would you handle memory leaks in Flutter?
* **Answer:** Cancel stream subscriptions and close controllers (e.g., `TextEditingController`, `AnimationController`, `StreamController`) within the widget's `dispose()` lifecycle method.

### 151. How would you improve app startup time?
* **Answer:** Use deferred loading for secondary packages, minimize initialization tasks in `main()`, optimize assets, and use Ahead-Of-Time (AOT) compilation parameters.

### 152. How would you debug a crashing Flutter app?
* **Answer:** Review local console logs, check trace paths in DevTools, and analyze remote error logs in Crashlytics.

### 153. How would you secure a production Flutter application?
* **Answer:** Apply code obfuscation (`--obfuscate`), implement SSL pinning, encrypt local databases, and store secrets securely using Keychain/Keystore wrapper libraries.

---

## Coding Questions

### 154. Write a Dio interceptor example.
* **Answer:**
  ```dart
  import 'package:dio/dio.dart';

  class CustomLoggingInterceptor extends Interceptor {
    @override
    void onRequest(RequestOptions options, RequestInterceptorHandler handler) {
      print("Outbound HTTP Request path: ${options.path}");
      super.onRequest(options, handler);
    }
  }
  ```

### 155. Write a simple Provider example.
* **Answer:**
  ```dart
  import 'package:flutter/foundation.dart';

  class CounterState extends ChangeNotifier {
    int _count = 0;
    int get count => _count;

    void increment() {
      _count++;
      notifyListeners();
    }
  }
  ```

### 156. Write pagination logic in Flutter.
* **Answer:**
  ```dart
  import 'package:flutter/material.dart';

  class ScrollListener {
    final ScrollController controller = ScrollController();
    final VoidCallback loadMore;

    ScrollListener({required this.loadMore}) {
      controller.addListener(_onScroll);
    }

    void _onScroll() {
      if (controller.position.pixels >= controller.position.maxScrollExtent - 200) {
        loadMore();
      }
    }
  }
  ```

### 157. Write a debounce search implementation.
* **Answer:**
  ```dart
  import 'dart:async';

  class Debouncer {
    final Duration delay;
    Timer? _timer;

    Debouncer({required this.delay});

    void run(VoidCallback action) {
      _timer?.cancel();
      _timer = Timer(delay, action);
    }
    
    void dispose() => _timer?.cancel();
  }
  ```

### 158. Write isolate example using compute().
* **Answer:**
  ```dart
  import 'package:flutter/foundation.dart';

  int parseLargeDataset(String rawData) {
    // Heavy processing task
    return rawData.length;
  }

  void processDataset() async {
    int length = await compute(parseLargeDataset, "large_raw_string_data");
    print("Processed string length: $length");
  }
  ```

### 159. Write API retry logic.
* **Answer:**
  ```dart
  import 'package:dio/dio.dart';

  Future<Response> fetchWithRetry(Dio dio, String url, {int retries = 3}) async {
    try {
      return await dio.get(url);
    } catch (e) {
      if (retries > 1) {
        return fetchWithRetry(dio, url, retries: retries - 1);
      }
      rethrow;
    }
  }
  ```

### 160. Write token refresh implementation.
* **Answer:**
  ```dart
  import 'package:dio/dio.dart';

  void setupTokenInterceptor(Dio dio) {
    dio.interceptors.add(InterceptorsWrapper(
      onError: (DioException error, ErrorInterceptorHandler handler) async {
        if (error.response?.statusCode == 401) {
          String newAccessToken = await refreshAccessToken();
          error.requestOptions.headers['Authorization'] = 'Bearer $newAccessToken';
          
          // Re-execute failed request
          final response = await dio.fetch(error.requestOptions);
          return handler.resolve(response);
        }
        return handler.next(error);
      },
    ));
  }

  Future<String> refreshAccessToken() async => "new_token";
  ```

### 161. Implement dark/light theme switching.
* **Answer:**
  ```dart
  import 'package:flutter/material.dart';

  class ThemeController extends ChangeNotifier {
    ThemeMode _themeMode = ThemeMode.light;
    ThemeMode get themeMode => _themeMode;

    void toggleTheme() {
      _themeMode = (_themeMode == ThemeMode.light) ? ThemeMode.dark : ThemeMode.light;
      notifyListeners();
    }
  }
  ```

### 162. Create a reusable custom widget.
* **Answer:**
  ```dart
  import 'package:flutter/material.dart';

  class ReusableActionButton extends StatelessWidget {
    final String label;
    final VoidCallback onPressed;

    const ReusableActionButton({
      required this.label,
      required this.onPressed,
      super.key,
    });

    @override
    Widget build(BuildContext context) {
      return ElevatedButton(
        onPressed: onPressed,
        child: Text(label),
      );
    }
  }
  ```

### 163. Implement pull-to-refresh functionality.
* **Answer:**
  ```dart
  import 'package:flutter/material.dart';

  Widget buildRefreshableList({
    required RefreshCallback onRefresh,
    required List<Widget> items,
  }) {
    return RefreshIndicator(
      onRefresh: onRefresh,
      child: ListView(
        children: items,
      ),
    );
  }
  ```
