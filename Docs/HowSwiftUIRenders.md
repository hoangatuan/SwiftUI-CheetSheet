
This notes in this markdown are taken from this amazing article by Michale Long: https://medium.com/swlh/deep-inside-views-state-and-performance-in-swiftui-d23a3a44b79

# Table of contents

1. What is `View`?


## What is `View`?

- In SwiftUI,`View` is not the resulting user interface view. It's just a struct that contains a simple description of what we want our user interface to look like when it's render
- It's struct & it's lightweight to init.

| A SwiftUI View is not a view. It's a description of a view.

## How SwiftUI renders?

- In SwiftUI, we use compositions to compose views.
- SwiftUI init the object, then call the `body` variable to determine what we want to build.
- SwiftUI continues the process until it goes to the end of the tree hierachy.

## What will happended when State changes?

| When State changes, any view with a direct dependency on that state will be regenerated

Example:

```swift

struct MasterView: View {
    @EnviromentObject var master: MasterViewModel
    var body: some View {
        ...
    }
}

```

1. Since MasterView depends on `master` => When `master` change, SwiftUI will call the `body` again.
2. SwiftUI will compare to decide which part need to be rerender (instead of rerender everything)

| When state changes, views downstream of that direct dependency may be also reinstantiated and regenerated.

## How to optimize performance?

- Since a view can be re-init & called the `body` when state change -> Avoid do heavy work on view initialization & in view body
- Keep the state change as low in hierachy as possible
- Avoid large collections of state data (Avoid Redux)
