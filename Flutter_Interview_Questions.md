# Flutter Developer Interview Questionnaire

## State Management
1. What state management techniques have you worked with in Flutter?
   * **Answer:** I have used tools like Provider, Riverpod, Bloc, and GetX. They are like different kinds of remote controls that help us update what's on the screen when we press a button.
2. Why would you choose GetX over Provider or Bloc?
   * **Answer:** Because GetX is super easy and fast to set up. It’s like a toy that doesn't need batteries or a manual; you just open the box and start playing!
3. What are the advantages and disadvantages of GetX?
   * **Answer:** 
     * *Advantages:* It is very fast and does many things (routing, state, dependencies) in one package.
     * *Disadvantages:* It can make the code messy if you aren't careful, like throwing all your toys into one giant box.
4. What is the difference between Provider, Riverpod, Bloc, and GetX?
   * **Answer:**
     * *Provider:* Like a helper that passes toys down to kids sitting below them.
     * *Riverpod:* A smarter helper that can pass toys anywhere without needing to stand on top of anyone.
     * *Bloc:* A strict rulebook where you must write down exactly what button you pressed and what happens next.
     * *GetX:* A magic wand that changes whatever you point at instantly.
5. Why is Riverpod considered an improvement over Provider?
   * **Answer:** Riverpod doesn't get confused about where a widget is placed on the screen, and it catches mistakes before you run the app—like a smart puzzle that won't let you put the wrong piece in.
6. What is the difference between reactive and non-reactive state management in GetX?
   * **Answer:** *Reactive* means the screen updates automatically the second a value changes (like a live video). *Non-reactive* means you have to manually tell the screen to redraw itself (like hitting refresh on a webpage).
7. Explain Rx variables in GetX.
   * **Answer:** They are "magic variables" that shout "Hey, I changed!" to the screen whenever their value updates, so the screen immediately updates itself.
8. What is the purpose of notifyListeners() in Provider?
   * **Answer:** It’s like a teacher clapping their hands to tell the whole class (all the widgets) to look at the board because something has changed.
9. When would you use Bloc instead of GetX?
   * **Answer:** When building a giant lego castle with a big team of builders. Bloc has strict rules that make sure nobody accidentally breaks someone else's work.
10. How does state persist after app refresh or app restart?
    * **Answer:** We save the state in a small digital notebook inside the phone (like SharedPreferences or Hive) so the app can read it again when it wakes up.

## Flutter Architecture
11. Which architecture pattern have you used in Flutter projects?
    * **Answer:** I use Clean Architecture. It is like organizing your room with separate boxes for clothes, toys, and books so nothing gets lost.
12. Explain Clean Architecture in Flutter.
    * **Answer:** It divides the app into three main parts: the look (Presentation), the rules (Domain), and the data (Data). This stops them from getting tangled up.
13. What are the responsibilities of Presentation, Domain, and Data layers?
    * **Answer:**
      * *Presentation:* The pretty screen and buttons you see and touch.
      * *Domain:* The brain and rules of the app (like: "you must be 10 years old to play").
      * *Data:* The backpack that gets information from the internet or a database.
14. What are Use Cases in Clean Architecture?
    * **Answer:** A Use Case is a single action the user can do, like "Log In" or "Add to Cart." It’s like a single recipe in a cookbook.
15. What is the role of Repository classes?
    * **Answer:** A Repository is like a waiter in a restaurant. You ask them for food (data), and they decide whether to get it from the kitchen (internet) or the fridge (local cache).
16. How do layers communicate with each other?
    * **Answer:** They talk using strict interfaces (rules). The screen talks to the brain, and the brain talks to the waiter. They never skip steps!
17. How would you structure a Flutter project with Login, Dashboard, and Profile?
    * **Answer:** I would create three main folders called `login`, `dashboard`, and `profile`. Inside each, I'd have folders for their screens, rules, and data.
18. Where would you place Providers, Bloc classes, APIs, Models, and Repositories?
    * **Answer:**
      * *Blocs/Providers:* In the Presentation folder (near the screens).
      * *APIs/Models:* In the Data folder (where the internet data comes in).
      * *Repositories:* In the Domain/Data folders to connect the brain to the data.
19. What are the benefits of feature-based folder structure?
    * **Answer:** If you want to fix the "Profile" screen, you only open the "Profile" folder. It's like having your toys sorted by play-sets instead of having all lego blocks in one box and all action figures in another.
20. How do you maintain scalability in large Flutter applications?
    * **Answer:** By keeping code small, using separate folders for each feature, and writing tests to make sure new updates don't break old features.

## Navigation & Session Handling
21. How do you manage navigation in Flutter?
    * **Answer:** Navigation is like turning pages in a book. We use GoRouter or Flutter's Navigator to move from one screen to another.
22. What is the difference between Navigator and GetX navigation?
    * **Answer:** *Navigator* needs a special context (like a map) to find the next screen. *GetX* navigation can jump to any screen instantly without needing that map.
23. How do you restore navigation state after refresh?
    * **Answer:** On the web, we use URLs (like `myapp.com/profile`) so the browser knows exactly which page to open when you hit refresh.
24. How do you persist session data in Flutter?
    * **Answer:** We save a secret key (token) in the phone's secure memory (like a locked diary) so the app remembers who you are even if you close it.
25. What happens when a user refreshes a Flutter web application?
    * **Answer:** The app completely restarts from scratch, like closing a book and opening it again. We must use URLs or local storage to remember where they were.
26. How would you save the last visited screen?
    * **Answer:** Write the name of the last screen in a mini-notebook (SharedPreferences) every time the user moves, and read it when the app starts.

## Async Programming
27. What is the difference between FutureBuilder and StreamBuilder?
    * **Answer:** *FutureBuilder* waits for a single package to arrive (like a birthday gift). *StreamBuilder* keeps waiting for a continuous flow of packages (like a water pipe dripping water).
28. What is the difference between BlocBuilder and FutureBuilder?
    * **Answer:** *FutureBuilder* is for one-time events (like fetching a profile photo). *BlocBuilder* is for listening to complex changes over and over (like game characters moving around).
29. When should you use FutureBuilder?
    * **Answer:** When you ask the internet for one thing, like "What is the weather right now?" and wait for the single answer.
30. When should you use StreamBuilder?
    * **Answer:** When you want real-time updates, like watching a live chat message screen or a stopwatch ticking.
31. How do you handle API loading, success, and error states?
    * **Answer:** We show a loading spinner (waiting), the actual data (success), or a sad face with a "Try Again" button (error).

## Flutter Web
32. Have you worked on Flutter Web projects?
    * **Answer:** Yes! It lets us run the same Flutter code inside a web browser like Chrome, instead of just on a phone.
33. What are the challenges in Flutter Web?
    * **Answer:** It can load slowly at first, some phone plugins don't work, and making things look good on giant computer monitors takes extra planning.
34. How do you make Flutter Web responsive?
    * **Answer:** We check how wide the screen is. If it's wide (PC), we show columns side-by-side. If it's narrow (phone), we stack them on top of each other.
35. What is the difference between MediaQuery and LayoutBuilder?
    * **Answer:** *MediaQuery* tells you the size of the whole screen. *LayoutBuilder* tells you the size of the specific box your widget is allowed to draw in.
36. How do you optimize Flutter Web performance?
    * **Answer:** By using smaller images, removing unused code (tree shaking), and choosing the right renderer (HTML for fast loading, CanvasKit for pretty graphics).
37. How do you handle browser refresh in Flutter Web?
    * **Answer:** By saving important data to the browser's local storage and using GoRouter to keep the web address (URL) correct.

## File Upload & API Integration
38. How do you upload files in Flutter?
    * **Answer:** We pick a file from the phone and send it to the internet using a package like Dio, which acts like a mail carrier delivering a package.
39. How do you upload files in Flutter Web?
    * **Answer:** We read the file as bytes (numbers) because web browsers don't let apps touch the computer's direct files for safety reasons.
40. What is multipart/form-data?
    * **Answer:** It is a special way of packing files and text together in one box so they can travel across the internet safely.
41. What packages have you used for file upload?
    * **Answer:** I use `file_picker` to choose the file, and `dio` or `http` to send it up to the server.
42. How do you handle platform-specific implementations in Flutter?
    * **Answer:** We use conditional imports or write native code (Java/Swift) and connect them using Platform Channels (a walkie-talkie between Flutter and the phone).
43. What is kIsWeb in Flutter?
    * **Answer:** It’s a boolean flag that is `true` if the app is running in a browser, and `false` if it is running on a phone.

## Dependency & Deployment
44. What happens when a new package dependency is added?
    * **Answer:** Flutter downloads the new package and adds it to our project's toolbox so we can use its pre-written code.
45. What issues can occur after deploying to production?
    * **Answer:** The app might crash on older phones, run slowly, or fail to talk to the internet if the server settings changed.
46. How do you manage package versions in Flutter?
    * **Answer:** We use the `pubspec.yaml` file to list our packages and their version numbers, like a shopping list for our app's tools.
47. What is dependency conflict?
    * **Answer:** It's when package A needs package C version 1.0, but package B needs package C version 2.0. The app gets confused because it can't use two different versions of C at the same time.
48. How do you debug production issues?
    * **Answer:** We use crash reporting tools like Firebase Crashlytics which send us reports showing exactly where the app crashed.

## Flutter Fundamentals
49. What is the widget lifecycle in Flutter?
    * **Answer:** It's the life story of a widget: it is born (`initState`), it shows up on screen (`build`), it changes when updated, and it dies (`dispose`).
50. Difference between StatelessWidget and StatefulWidget?
    * **Answer:** *Stateless* is like a printed picture (never changes). *Stateful* is like a digital screen (can change its picture when you tap it).
51. What are Keys in Flutter?
    * **Answer:** They are unique ID tags for widgets. They help Flutter keep track of which widget is which when they move around the screen.
52. What is BuildContext?
    * **Answer:** It is like a map showing a widget's exact family tree and where it is located inside the giant tree of widgets.
53. What causes unnecessary widget rebuilds?
    * **Answer:** When you tell a parent widget to redraw itself, it often redraws all its kids too, even if the kids didn't change at all.
54. How do you optimize Flutter app performance?
    * **Answer:** Use `const` widgets, avoid rebuilds, make images smaller, and don't do heavy math inside the `build()` method.
55. What are mixins in Dart?
    * **Answer:** They are like badge skills (e.g., swimming, flying). A class can use a mixin to get those skills without needing to inherit from a parent class.
56. What are isolates in Flutter?
    * **Answer:** They are separate little workers inside the phone's CPU. They run code on their own thread so the main screen thread never freezes.

## Practical & Scenario-Based Questions
57. How would you design a scalable e-commerce Flutter app?
    * **Answer:** I would split it into features (cart, products, auth) and use Clean Architecture so multiple developers can work on it without stepping on each other's toes.
58. Suppose the API fails — how would your UI behave?
    * **Answer:** It should show a friendly error page (like a picture of a sleeping dog) and a "Try Again" button, instead of showing a blank white screen.
59. How would you implement offline support?
    * **Answer:** By saving data in Hive or SQLite. When the user opens the app, we show the saved data first before trying to download new data.
60. How would you handle app state after internet reconnect?
    * **Answer:** We listen to the network status. Once the internet is back, we automatically refresh the screen and send any saved offline actions to the server.
61. How would you secure sensitive data in Flutter?
    * **Answer:** By using `flutter_secure_storage` which hides keys and passwords in the phone's highly guarded keychain system.
62. How would you handle deep linking?
    * **Answer:** We configure the phone to open our app when a user clicks a link (like `myapp://product/123`) and route them directly to that product.
63. Explain a challenging bug you solved recently.
    * **Answer:** An image list was scrolling very slowly. I fixed it by making the images smaller before loading them and using lazy loading so only visible images loaded.
64. Explain a performance issue you faced in Flutter and how you fixed it.
    * **Answer:** A screen rebuilded 60 times a second. I fixed it by moving the changing part into its own small widget and using a Provider selector.
65. If your app crashes only in production, how would you debug it?
    * **Answer:** I would check Firebase Crashlytics logs to see the stack trace and test on the exact phone model that reported the crash.

## Team & Project Experience
66. How do you collaborate with backend teams?
    * **Answer:** We agree on the API structure using documentation tools like Swagger so both teams know what the requests and responses will look like.
67. How do you manage code reviews?
    * **Answer:** We use Git Pull Requests. Other developers look at the code, suggest improvements, and check for bugs before merging it.
68. Have you worked in Agile/Scrum?
    * **Answer:** Yes, we break work into 2-week "sprints," plan our goals together, and have quick daily meetings to talk about what we are doing.
69. How do you handle merge conflicts?
    * **Answer:** I compare both versions of the code line-by-line, keep the correct parts of both, and test the app to make sure nothing broke.
70. What role do you usually play in your project team?
    * **Answer:** I am the frontend Flutter developer who turns the designer's drawings into a real, working app that users can touch and interact with.

## API & Networking
71. What is the difference between REST API and GraphQL?
    * **Answer:** *REST* is like a fixed combo meal (you get everything on the plate). *GraphQL* is like a buffet where you choose exactly what food you want on your plate.
72. How do you handle multiple API calls simultaneously in Flutter?
    * **Answer:** We use `Future.wait()`. It fires all the API calls together like kids running a race, and waits until everyone crosses the finish line.
73. What is Dio in Flutter and why is it preferred over HTTP package?
    * **Answer:** Dio is like a super-powered mail truck. It can cancel deliveries, retry automatically, and intercept mail, whereas HTTP is just a standard mailbox.
74. What are interceptors in Dio?
    * **Answer:** They are like security guards at a gate who check, log, or change letters (requests) before they leave the app or when they return from the internet.
75. Explain onRequest, onResponse, and onError in Dio interceptors.
    * **Answer:**
      * *onRequest:* Runs before the request leaves the app (like putting a stamp on a letter).
      * *onResponse:* Runs when data arrives back (like inspecting the package).
      * *onError:* Runs if something went wrong (like handling a "return to sender" envelope).
76. How do you add authentication tokens using interceptors?
    * **Answer:** The guard (`onRequest`) automatically stamps the secret passcode (token) onto every request header before sending it out.
77. How do you refresh expired JWT tokens automatically?
    * **Answer:** If an API returns a "401 Expired" error, the interceptor pauses the request, calls a refresh API to get a new token, and retries the original request with it.
78. What is global error handling in Flutter networking?
    * **Answer:** A central catch-all system that shows a popup or message whenever any API call fails, so we don't have to write error logic on every screen.
79. How do you handle API timeout scenarios?
    * **Answer:** We set a timer (e.g., 10 seconds). If the server doesn't respond by then, we stop waiting and tell the user "The connection took too long."
80. How do you implement retry mechanisms for failed APIs?
    * **Answer:** If an API fails due to bad internet, we wait a second and try sending it again (up to 3 times) before giving up.
81. What is middleware? How is it different from interceptors?
    * **Answer:** *Middleware* acts like a ticket gate before you enter a screen. *Interceptors* are guards that check network requests.
82. Where are middleware commonly used?
    * **Answer:** To check if a user is logged in before letting them open the "Dashboard" screen. If they aren't, it sends them back to the "Login" screen.
83. Can interceptors modify request headers and responses?
    * **Answer:** Yes, they can add things to headers (like tokens) or modify responses (like converting raw text to objects) before the app sees them.
84. How do you log API requests and responses in Flutter?
    * **Answer:** We use loggers like `PrettyDioLogger` to print clean, easy-to-read text in our console showing exactly what we sent and received.
85. What is request cancellation in Dio?
    * **Answer:** It’s like cancelling an order before the delivery truck arrives. If the user leaves a screen, we cancel its ongoing API requests to save data.
86. How do you upload files/images using Dio?
    * **Answer:** We wrap the file in a `MultipartFile` inside a `FormData` object and post it to the server.
87. How do you download large files efficiently in Flutter?
    * **Answer:** We use Dio's `download()` method and listen to the progress percentage so we can show a progress bar to the user.
88. How do you secure API calls in Flutter applications?
    * **Answer:** By using HTTPS, locking the app's SSL certificates (SSL Pinning), and hiding API keys safely in native configuration files.

## Additional State Management
89. Which state management approaches have you used in Flutter?
    * **Answer:** I have used tools like Provider, Riverpod, Bloc, and GetX. They are like different kinds of remote controls that help us update what's on the screen when we press a button. *(Same as Q1)*
90. Difference between Provider, Bloc, Riverpod, and GetX?
    * **Answer:**
      * *Provider:* Like a helper that passes toys down to kids sitting below them.
      * *Riverpod:* A smarter helper that can pass toys anywhere without needing to stand on top of anyone.
      * *Bloc:* A strict rulebook where you must write down exactly what button you pressed and what happens next.
      * *GetX:* A magic wand that changes whatever you point at instantly. *(Same as Q4)*
91. What is the difference between setState and Provider?
    * **Answer:** *setState* is like painting a single widget. *Provider* is like broadcast television; it sends updates to multiple widgets across the app.
92. When would you choose Bloc over Provider?
    * **Answer:** When you have a complex app where you want to separate *what happened* (events) from *what changes on screen* (states).
93. Explain reactive programming in Flutter.
    * **Answer:** It is like building a row of dominoes. When one piece changes, it triggers a chain reaction that automatically updates everything else connected to it.
94. What are streams in Dart?
    * **Answer:** They are like conveyor belts. Objects keep coming down the belt one by one, and we can watch and grab them as they pass.
95. Difference between StreamBuilder and FutureBuilder?
    * **Answer:** *FutureBuilder* waits for a single package to arrive (like a birthday gift). *StreamBuilder* keeps waiting for a continuous flow of packages (like a water pipe dripping water). *(Same as Q27)*

## Flutter Core Concepts
96. Difference between StatelessWidget and StatefulWidget?
    * **Answer:** *Stateless* is like a printed picture (never changes). *Stateful* is like a digital screen (can change its picture when you tap it). *(Same as Q50)*
97. What is widget lifecycle in Flutter?
    * **Answer:** It's the life story of a widget: it is born (`initState`), it shows up on screen (`build`), it changes when updated, and it dies (`dispose`). *(Same as Q49)*
98. Difference between initState(), build(), and dispose()?
    * **Answer:**
      * *initState():* Born (setting up things once).
      * *build():* Drawing (showing the widget on the screen).
      * *dispose():* Goodbye (cleaning up memory so the phone doesn't slow down).
99. What are keys in Flutter?
    * **Answer:** They are unique ID tags for widgets. They help Flutter keep track of which widget is which when they move around the screen. *(Same as Q51)*
100. Difference between GlobalKey and ValueKey?
     * **Answer:** *ValueKey* is a local tag (like labeling a toy in a drawer). *GlobalKey* is a unique tag across the whole app (like a GPS tracker on a toy).
101. What is BuildContext?
     * **Answer:** It is like a map showing a widget's exact family tree and where it is located inside the giant tree of widgets. *(Same as Q52)*
102. What causes widget rebuilds?
     * **Answer:** When a widget's parent rebuilds, when `setState()` is called, or when shared data it is listening to changes.
103. How do you optimize Flutter app performance?
     * **Answer:** Use `const` widgets, avoid rebuilds, make images smaller, and don't do heavy math inside the `build()` method. *(Same as Q54)*
104. What is const constructor optimization?
     * **Answer:** It tells Flutter: "This widget will never change." Flutter then saves it in memory and reuses it instead of creating it again.
105. What are mixins in Dart?
     * **Answer:** They are like badge skills (e.g., swimming, flying). A class can use a mixin to get those skills without needing to inherit from a parent class. *(Same as Q55)*
106. Difference between final and const?
     * **Answer:** *const* must be known when writing the code (like the value of Pi). *final* is set once when the app is running (like the current time).
107. What are extensions in Dart?
     * **Answer:** They let us add new functions to existing classes (like adding a helper method to `String` or `DateTime`) without changing their original files.

## Isolates & Performance
108. What are isolates in Flutter?
     * **Answer:** They are separate little workers inside the phone's CPU. They run code on their own thread so the main screen thread never freezes. *(Same as Q56)*
109. Why do we use isolates?
     * **Answer:** To run heavy code (like loading a giant file) on a separate track so the screen doesn't freeze or stutter.
110. Difference between async/await and isolates?
     * **Answer:** *async/await* is one worker taking turns between tasks. *Isolates* are two separate workers doing tasks at the same time.
111. What tasks should be moved to isolates?
     * **Answer:** Complex image editing, decoding huge JSON files, or sorting millions of numbers.
112. Explain compute() in Flutter.
     * **Answer:** It is a simple shortcut to send a heavy job to another worker (isolate) and get the result back easily.
113. How do isolates communicate with each other?
     * **Answer:** They send messages to each other using "ports" (like sending walkie-talkie messages). They don't share memory.
114. What happens if heavy processing runs on the main thread?
     * **Answer:** The screen lags and freezes because the main worker is too busy doing math to redraw the screen.
115. Difference between concurrency and parallelism?
     * **Answer:** *Concurrency* is one worker switching back and forth between two tasks. *Parallelism* is two workers doing two tasks at the exact same time.

## Local Storage & Caching
116. Difference between SharedPreferences, Hive, and SQLite?
     * **Answer:**
       * *SharedPreferences:* A small note for simple things (like: "is dark mode on?").
       * *Hive:* A fast storage box for saving objects and lists.
       * *SQLite:* A large filing cabinet with indexes for massive tables of data.
117. How do you cache API responses?
     * **Answer:** We save the internet response inside local storage (like Hive). If the internet is down, we read from this local box.
118. What is offline-first architecture?
     * **Answer:** Designing the app so it always shows stored local data first, and only uses the internet to fetch updates behind the scenes.
119. How do you store sensitive data securely in Flutter?
     * **Answer:** By encrypting the data and using keychain wrappers like `flutter_secure_storage` to keep it safe from hackers.
120. What is Flutter Secure Storage?
     * **Answer:** A package that talks to the phone's safest vault (Keychain on iOS, Keystore on Android) to store keys and passwords securely.

## Architecture
121. Which architecture patterns have you used?
     * **Answer:** Clean Architecture, MVVM, and MVC. *(Same as Q11)*
122. MVC
     * **Answer:** *Model-View-Controller:* Model is the data, View is the screen, and Controller is the bridge that tells the View what data to show.
123. MVVM
     * **Answer:** *Model-View-ViewModel:* Similar to MVC, but the ViewModel prepares data specifically for the screen to display easily.
124. Clean Architecture
     * **Answer:** It divides the app into three main parts: the look (Presentation), the rules (Domain), and the data (Data). This stops them from getting tangled up. *(Same as Q12)*
125. Explain Clean Architecture in Flutter.
     * **Answer:** Presentation holds the widgets, Domain holds the business rules, and Data handles database and internet connections. *(Same as Q13)*
126. What is repository pattern?
     * **Answer:** A pattern that hides *where* the data comes from (database or internet), giving the app a clean way to request data.
127. What is dependency injection?
     * **Answer:** Instead of a class making its own tools, we pass the tools to it when it is created (like giving a builder their hammer).
128. Have you used GetIt or Injectable?
     * **Answer:** Yes, GetIt is like a toolbox locator. You register your tools in it once, and any class can grab those tools whenever they need them.
129. Difference between tightly coupled and loosely coupled code?
     * **Answer:** *Tightly coupled* is like Lego blocks glued together (hard to separate). *Loosely coupled* is like normal Lego blocks (easy to swap or replace).

## Firebase
130. Have you integrated Firebase in Flutter?
     * **Answer:** Yes, Firebase is a suite of backend tools by Google that handles databases, logins, and notifications easily.
131. Difference between Firebase Authentication and Firestore?
     * **Answer:** *Authentication* checks passwords and logins. *Firestore* is the database where we store app text, messages, or user profiles.
132. How do push notifications work in Flutter?
     * **Answer:** A server sends a message to Google/Apple servers, which deliver it to the phone. The phone wakes up our app and shows the message.
133. What is Firebase Cloud Messaging (FCM)?
     * **Answer:** A free service from Firebase that lets us send notifications to users' devices.
134. How do you handle background notifications?
     * **Answer:** We write a top-level function that runs in the background when the app is closed, letting the phone process the message.

## Advanced Flutter
135. What are platform channels?
     * **Answer:** A bridge (walkie-talkie) that lets Flutter Dart code talk to native Android (Kotlin) or iOS (Swift) code to use device features.
136. How does Flutter communicate with native Android/iOS code?
     * **Answer:** By sending messages back and forth through MethodChannels (for one-time events) or EventChannels (for streams of data).
137. Difference between Future and Stream?
     * **Answer:** *Future* is one single event (like a package in the mail). *Stream* is a series of events (like watching a movie stream).
138. What is code splitting?
     * **Answer:** Breaking a big app bundle into smaller chunks so the user only downloads the parts they need when they need them.
139. What is tree shaking in Flutter?
     * **Answer:** When building the app, Flutter shakes off any unused code or packages like dry leaves, making the final app size smaller.
140. What are flavors/environments in Flutter?
     * **Answer:** Different versions of the app, like "Dev" for testing (sandbox) and "Prod" for real users (live store).
141. How do you manage dev, staging, and production builds?
     * **Answer:** We use schemes/flavors to point the app to different server databases (like a testing server vs a real server).
142. What are deep links?
     * **Answer:** Links (like `https://myapp.com/item/5`) that open your app on the phone instead of opening a web browser.
143. What is app lifecycle in Flutter?
     * **Answer:** The states the app goes through: active (foreground), inactive (paused), or hidden (background).

## Practical Scenarios
144. How would you design a dashboard with 5 simultaneous API calls?
     * **Answer:** I would fire them all together using `Future.wait()`, show a loading spinner, and update the screen once they all finish.
145. How would you avoid duplicate API calls during screen rebuild?
     * **Answer:** Store the Future in a state variable inside a `StatefulWidget`'s `initState()` so it only fetches once and doesn't rerun on rebuild.
146. How would you optimize a slow Flutter app?
     * **Answer:** By removing large rebuilds, caching images, using `const` constructors, and running heavy tasks on Isolates.
147. How would you handle session expiration globally?
     * **Answer:** By listening to API responses. If we get a "token expired" (401) error, we log out the user and send them to the login screen.
148. How would you implement pagination?
     * **Answer:** Listen to the scroll position. When the user reaches the bottom, we fetch the next page of items and add them to the list.
149. How would you implement search with debounce?
     * **Answer:** We wait for the user to stop typing for a brief moment (e.g., 500ms) before sending the search query to the internet, so we don't overload the server.
150. How would you handle memory leaks in Flutter?
     * **Answer:** Always close controllers (like `TextEditingController` or `StreamController`) in the `dispose()` method when the widget dies.
151. How would you improve app startup time?
     * **Answer:** Use deferred loading, avoid heavy tasks in `main()`, and optimize the size of assets (like logos and fonts).
152. How would you debug a crashing Flutter app?
     * **Answer:** Read the debug console logs, use the Flutter DevTools Inspector, and look at the crash reports in Firebase Crashlytics.
153. How would you secure a production Flutter application?
     * **Answer:** Obfuscate code, use secure storage, encrypt local databases, and implement SSL pinning to secure internet traffic.

## Coding Questions
154. Write a Dio interceptor example.
     * **Answer:** Here is a simple guard interceptor that logs requests:
       ```dart
       class LogInterceptor extends Interceptor {
         @override
         void onRequest(RequestOptions options, RequestInterceptorHandler handler) {
           print("Sending mail to: ${options.path}");
           super.onRequest(options, handler);
         }
       }
       ```
155. Write a simple Provider example.
     * **Answer:** Here is a counter box that tells widgets to rebuild:
       ```dart
       class Counter extends ChangeNotifier {
         int value = 0;
         void increment() {
           value++;
           notifyListeners(); // Tells the children to update!
         }
       }
       ```
156. Write pagination logic in Flutter.
     * **Answer:** A list controller that checks if we are at the bottom:
       ```dart
       void _onScroll() {
         if (_scrollController.position.pixels == _scrollController.position.maxScrollExtent) {
           _fetchNextPage(); // Get more items!
         }
       }
       ```
157. Write a debounce search implementation.
     * **Answer:** A timer that waits before searching:
       ```dart
       Timer? _debounce;
       void _onSearchChanged(String text) {
         if (_debounce?.isActive ?? false) _debounce?.cancel();
         _debounce = Timer(Duration(milliseconds: 500), () {
           _searchApi(text); // Search after 500ms of quiet time!
         });
       }
       ```
158. Write isolate example using compute().
     * **Answer:** Sending math to a helper cook:
       ```dart
       int heavyMath(int value) => value * 2; // Hard math
       void run() async {
         int result = await compute(heavyMath, 100);
         print(result);
       }
       ```
159. Write API retry logic.
     * **Answer:** Try sending the mail again if it failed:
       ```dart
       Future<Response> getWithRetry(String url, {int retries = 3}) async {
         try {
           return await dio.get(url);
         } catch (e) {
           if (retries > 1) return getWithRetry(url, retries: retries - 1);
           rethrow;
         }
       }
       ```
160. Write token refresh implementation.
     * **Answer:** Get a new key when the old key expires:
       ```dart
       if (error.response?.statusCode == 401) {
         String newToken = await refreshOldToken();
         // Retry the failed request with the new key!
         return handler.resolve(await dio.fetch(error.requestOptions..headers['Authorization'] = newToken));
       }
       ```
161. Implement dark/light theme switching.
     * **Answer:** Switch the theme style sheet:
       ```dart
       class ThemeProvider extends ChangeNotifier {
         ThemeMode mode = ThemeMode.light;
         void toggleTheme() {
           mode = (mode == ThemeMode.light) ? ThemeMode.dark : ThemeMode.light;
           notifyListeners();
         }
       }
       ```
162. Create a reusable custom widget.
     * **Answer:** A Lego block button you can reuse anywhere:
       ```dart
       class CoolButton extends StatelessWidget {
         final String text;
         final VoidCallback onTap;
         const CoolButton({required this.text, required this.onTap, super.key});
         @override
         Widget build(BuildContext context) => ElevatedButton(onPressed: onTap, child: Text(text));
       }
       ```
163. Implement pull-to-refresh functionality.
     * **Answer:** Wrap your list in a `RefreshIndicator`:
       ```dart
       RefreshIndicator(
         onRefresh: () async => await fetchLatestData(),
         child: ListView(children: [...]),
       )
       ```
