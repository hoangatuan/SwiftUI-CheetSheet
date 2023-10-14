
# Table of Contents

1. AppStorage
2. SwiftData

## AppStorage

The @AppStorage Property Wrapper was introduced in SwiftUI to allow easy access to the user defaults.

```swift
    @AppStorage("username") var username: String = "Anonymous"
    @AppStorage("should_show_hello_world", store: .standard) var shouldShowHelloWorld: Bool = false
```

> The app storage property wrapper *has a projected value providing a binding* that you can use with SwiftUI controls.

- @AppStorage will watch UserDefaults.standard by default
- You can create a custom UserDefaullt storage:

```swift
    @AppStorage("username", store: UserDefaults(suiteName: "group.com.hackingwithswift.unwrap"))
```

### References:

- https://www.avanderlee.com/swift/appstorage-explained/

## SwiftData

### Introduction

- SwiftData is a fast, powerful, and easy-to-use way to store data in apps built for iOS, macOS, tvOS, watchOS, and even visionOS
- Behind the scenes, SwiftData is powered by a much bigger and more mature framework called Core Data.
- Support iOS 17+
- Many features on CoreData not yet supported on SwiftData

### How to use

1. Model your data using `@Model`.*SwiftData will automaticallly infer relationships*

```
@Model
class Recipe {
    @Attribute(.unique) var name: String
    var summary: String?
    
     @Relationship(deleteRule: .cascade, inverse: \Ingredient.recipe) // Set up deletion rule
    var ingredients: [Ingredient]
}
```

2. Create container

```swift
    // This is the func to create container: modelContainer(for:inMemory:isAutosaveEnabled:isUndoEnabled:onSetup:)
    
    .modelContainer(for: Recipe.self) // By default, isAutosaveEnabled = true -> so it willl autosave
```

3. Create, detele, update

```swift
    @Environment(\.modelContext) private var context // Define the context
    
    ... // To add
        let recipe = Recipe(...)
        context.insert(recipe)
    ...
    
    ... // To delete
        context.delete(item)
    ...
    
    ... // To update
        recipe.summary = ...
        try? context.save() // Call this to save
    ...
```

4. Use `@Query` in SwiftUI view to fetch data. It will *automatically update UI when data change*

```swift
@Query var recipes: [Recipe]
var body: some View {
    List(recipes) { recipe in
        NavigationLink(recipe.name, destination: RecipeView(recipe))
    }
}
```

- Using predicate:

```swift
let simpleFood = #Predicate<Recipe> { recipe in
    recipe.ingredients.count < 3
}
```

### References:

- https://www.hackingwithswift.com/quick-start/swiftdata
- https://developer.apple.com/xcode/swiftdata/
- https://developer.apple.com/documentation/swiftdata/deleting-persistent-data-from-your-app
