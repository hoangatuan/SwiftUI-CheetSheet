# Table of Contents

1. @State
2. @Binding
3. @StateObject
4. @ObservableObject & @Obsereved
5. @EnviromentObject
6. @Observable, @Enviroment, @Bindable

## @State

| @State: A property wrapper type that can read and write a value managed by SwiftUI.

- State is the simplest source of truth your app can have. It is designed to contain **simple value types, such as Ints, Strings, and Bools**.
- When that state changes, SwiftUI knows to automatically reload the view with the latest changes so it can reflect its new information.
- @State is great for simple properties that belong to a specific view and never get used outside that view, so as a result **it’s important to mark those properties as being private** to re-enforce the idea that such state is specifically designed never to escape its view

```swift
    struct ContentView: View {
        @State private var tapCount = 0

        var body: some View {
            Button("Tap count: \(tapCount)") {
                tapCount += 1
            }
        }
    }
```

- Init `@State` with some value:

```swift
    struct Foo: View {
        @State private var flag: Bool
        
        init(flag: Bool) {
            _flag = .init(wrappedValue: flag)   // this is the conventional way to initialize @State in init
        }
            
        var body: some View {
            Text("Hello, world!")
        }

    }
```

### References

- [Apple document](https://developer.apple.com/documentation/swiftui/state)

## @Binding

| A property wrapper type that can read and write a value owned by a source of truth

- A binding is, like its name implies, a connection between 2 things. In SwiftUI, a binding sits between a property that stores data, and a view that displays and changes that data

- Init `@Binding`:

```swift
    init(message: String, isAccepted: Binding<Bool>) {
        self.message = message
        self._isAccepted = isAccepted

    }
```

### References:
- https://sarunw.com/posts/binding-initialization/


## ObservableObject & @Obsereved

### @ObservableObject

- This protocol stands for an object with a publisher that emits before the object has changed and allows you to tell SwiftUI to trigger a view redraw
- ObservableObject is very similar to `@State` except now we’re using an complex reference type rather than a simple local property.
- To mark a proterty in `ObservableObject` can makes the view to redraw when it changes, we need to mark it as `@Published`.

> Warning: When you use a custom publisher to announce that your object has changed, this **must happen on the main thread**.

### @ObservedObject

- Require your object to conform to the `ObservableObject` protocol. 

```swift
final class CounterViewModel: ObservableObject {
    @Published var count = 0

    func incrementCounter() {
        count += 1
    }
}

struct CounterView: View {
    @ObservedObject var viewModel = CounterViewModel()

    var body: some View {
        VStack {
            Text("Count is: \(viewModel.count)")
            Button("Increment Counter") {
                viewModel.incrementCounter()
            }
        }
    }
}
```

## @StateObject

- Require your object to conform to the `ObservableObject` protocol. (works kind of similar to @ObservedObject)

What's the difference between @StateObject & @ObservedObject?   
- Observed objects marked with the `@StateObject` **don’t get destroyed and re-instantiated at times their containing view struct redraws**

### References

- [Difference between @StateObject and @ObservedObject](https://www.avanderlee.com/swiftui/stateobject-observedobject-differences/)

## @EnviromentObject

- it’s shared data that every view can read if they want to.

Ex:
If your app had some important model data that all views needed to read, instead of handing it from view to view to view, you can just put it into the environment where every view has instant access to it.

## @Observable, @Enviroment, @Bindable

Introduced in WWDC23, `@Observable` is a new macro to simplifying observation related code and improving the performance of the app.

- The @Bindable property wrapper allows you to create bindings for properties that are owned by Observable classes. As mentioned earlier,@Bindable is limted to classes that conform to Observable and the easiest way to make Observable objects is the @Observable macro.

Before

```swift
    struct ContentView2: View {
        @EnvironmentObject var library: Library
        @ObservedObject/@StateObject private var viewModel = Content2ViewModel()

        var body: some View {
            ...
        }
    }

    class ContentViewModel: ObservabledObject {
        var username: String = ""

        var title: String {
            if username.isEmpty {
                return "Hello, world!"
            }
            else {
                return "Hello \(username)!"
            }
        }
    }
```

After

```swift
    struct ContentView2: View {
        @Environment(Library.self) private var library
        @State/@Bindable private var viewModel = Content2ViewModel()

        var body: some View {
            ...
        }
    }

    @Observable ContentViewModel {
        var username: String = ""

        var title: String {
            if username.isEmpty {
                return "Hello, world!"
            }
            else {
                return "Hello \(username)!"
            }
        }
    }
```

### Why @Observable has better performance compared to @ObservableObject?

- With @ObservableObject, all views which have a direct dependency to the @ObservableObject will be redraw when a @Published property change, even if the view doesn't use that @ObsevableObject
- WIth @Observable, only part that uses `@Observable` will be redraw

### Testing?

- https://betterprogramming.pub/unit-test-the-observation-framework-d0f0fe240944

### References:

- [Migrate ObservableObject to Observable by Apple](https://developer.apple.com/documentation/swiftui/migrating-from-the-observable-object-protocol-to-the-observable-macro) https://developer.apple.com/documentation/swiftui/migrating-from-the-observable-object-protocol-to-the-observable-macro
- [@Binding vs @Bindable](https://www.donnywals.com/whats-the-difference-between-binding-and-bindable/)
- [Deep dive to @Observable](https://betterprogramming.pub/a-deep-dive-into-observation-a-new-way-to-boost-swiftui-performance-f299831c664b)
