# MyPortfolio Mobile

A Flutter mobile application for managing a personal portfolio — built as part of the PBP (Platform-Based Programming) course at Fasilkom UI, Semester Gasal 2026/2027.

## Setup

1. Make sure Flutter SDK is installed and `flutter` is on your `PATH`.
2. Clone this repository.
3. Run `flutter pub get` to install dependencies.
4. Run `flutter run -d chrome` to launch in the browser, or `flutter run` for a connected device/emulator.

---

### Tugas 7

1. The **widget tree** is the hierarchical structure of all widgets that make up a Flutter UI. Every widget has a parent (except the root) and can have one or more children, forming a tree. Flutter traverses this tree top-down during the `build` phase: a parent widget calls `build()` which returns its child widgets, and those children do the same recursively. For example, `MaterialApp` is the root; it contains `MyHomePage` (a `Scaffold`), which contains an `AppBar` and a `body` made of `Padding` → `Column` → `Row` + `GridView`, and so on. The parent controls layout constraints and passes them down; children report their sizes back up. This parent-child relationship determines both rendering order and how context (like theme colors) flows down the tree.

2. Widgets used in this project and their functions:
   - `MaterialApp` — root widget; sets up routing, theme, and title for the whole app.
   - `ThemeData` / `ColorScheme.fromSeed` — defines the app-wide color palette derived from a seed color.
   - `MyHomePage` (`StatelessWidget`) — the main home page; assembles all other widgets.
   - `Scaffold` — provides the standard page structure: AppBar slot, body slot, and SnackBar support.
   - `AppBar` — the top navigation bar showing the app title.
   - `Padding` — adds uniform spacing around its child widget.
   - `Column` — arranges children vertically.
   - `Row` — arranges children horizontally (used for the three `InfoCard`s).
   - `GridView.count` — displays `ItemCard`s in a fixed 3-column grid.
   - `InfoCard` (custom `StatelessWidget`) — a `Card` + `Container` + `Column` that shows a label and value.
   - `Card` — a Material surface with elevation/shadow.
   - `Container` — a box that applies width, padding, and holds a child.
   - `ItemCard` (custom `StatelessWidget`) — a `Material` + `InkWell` + `Column` that renders a tappable action button with icon and label.
   - `Material` — provides the colored background and border radius for each action card.
   - `InkWell` — detects taps and shows a ripple effect; triggers the `SnackBar`.
   - `Icon` — displays a Material icon.
   - `Text` — renders a string with optional styling.
   - `SizedBox` — adds fixed vertical or horizontal spacing.
   - `Center` — centers its child within the available space.
   - `MediaQuery` — queries screen dimensions to size `InfoCard` proportionally.
   - `ScaffoldMessenger` / `SnackBar` — shows a brief notification bar at the bottom of the screen.

3. `MaterialApp` is the top-level widget that wires together all the foundational services a Material Design app needs: it sets up the `Navigator` (routing stack), injects the `Theme` down the widget tree, configures the app title (shown in the OS task switcher), and handles locale/text direction. It is used as the root widget because every widget below it can then access `Theme.of(context)`, `Navigator.of(context)`, and `MediaQuery.of(context)` — all of which depend on the `InheritedWidget`s that `MaterialApp` places at the top of the tree. Without it, widgets like `Scaffold`, `AppBar`, and `SnackBar` would not render correctly because they expect a Material ancestor.

4. A `StatelessWidget` has no mutable state: its `build()` method depends only on the constructor arguments it received, and it never changes after being built. A `StatefulWidget` owns a separate `State` object that can call `setState()` to trigger a rebuild whenever internal data changes. Use `StatelessWidget` when the UI is purely a function of its inputs and will never need to update itself (e.g. `InfoCard`, `ItemCard`, `MyHomePage` in this project — they display fixed data). Use `StatefulWidget` when the widget needs to react to user interaction, timers, or async data that changes over time (e.g. a counter, a form with live validation, or a page that fetches data from an API).

5. `BuildContext` is an object that represents the location of a widget in the widget tree. Every `build()` method receives a `context` argument that points to the widget's node in the tree. It is important because it is the handle through which a widget can look up inherited data that lives above it in the tree — for example, `Theme.of(context)` walks up the tree to find the nearest `Theme` widget, and `ScaffoldMessenger.of(context)` finds the nearest `ScaffoldMessenger`. Without `context`, widgets would have no way to access shared resources like the theme, navigator, or media query. In `build()`, `context` is used every time we call `Theme.of(context).colorScheme.primary` (to get the theme color) or `MediaQuery.of(context).size.width` (to get the screen width for `InfoCard` sizing).

6. **Hot reload** injects updated Dart code into the running Dart VM and rebuilds the widget tree from the current state — it is nearly instantaneous and preserves the app's current state (e.g. which page you are on, what text is in a field). **Hot restart** fully restarts the Dart VM and the app from `main()`, discarding all state, but is still faster than a full cold start because it does not recompile native code. Hot reload is sufficient for most UI tweaks (changing colors, text, layout). You must use hot restart (or a full restart) when: you add a new asset or font, change `main()` or app-level initialization code, modify `initState()` logic, change native platform code (Android/iOS), or add a new package that requires native linking.
