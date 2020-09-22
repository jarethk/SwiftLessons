# Core Data

## AppDelegate
Set up a persistentContainer in your AppDelegate.  Why?  It is pretty central, early startup, and easy to access from the rest of your code. See (a)

```swift
    lazy var persistentContainer: NSPersistentContainer = {
        let container = NSPersistentContainer(name: "DataModel")
        container.loadPersistentStores { description, error in
            if let error = error {
                fatalError("Unable to load persistent stores: \(error)")
            }
        }
        return container
    }()

```

For more on AppDelegate read Apple's doc (b)

## Data Manager
From there, I like to set up a Manager type class to handle the details of working with the persistentContainer.

```swift
    private var appDelegate : AppDelegate?

    init() {
        guard let appDelegate = UIApplication.shared.delegate as? AppDelegate else {
            fatalError("AppDelegate invalid")
        }
        self.appDelegate = appDelegate
    }
```

### Creating New Entity
When creating a new entity use a helper function from your Manager such as the below sample...

```swift
    func initEnt() -> Entity {
        let moc = appDelegate!.persistentContainer.viewContext
        let newEnt = Entity(context: moc)
        ... code to initialize
        return newPlayer
    }
```

### Saving 
When you save, you save the entire container.  You could have a save method on the AppDelegate, but I prefer to put it in my Manager class

```swift
    func save() {
        let moc = appDelegate!.persistentContainer.viewContext
        guard moc.hasChanges else { return }
        do {
            try moc.save()
        } catch let error as NSError {
            print("Could not save. \(error), \(error.userInfo)");
            fatalError("Unresolved error \(error), \(error.userInfo)")
        }
    }
```

## References
 - [a](https://www.codementor.io/@francofantillo/coredata-and-data-persistence-in-ios-uapgfgbxn)
 - [b](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)
