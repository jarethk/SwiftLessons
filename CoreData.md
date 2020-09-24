# Core Data

## Modeling

## CodeGen
CodeGen can seem great, but you lose some level of control (c).  Code generated with CodeGen set to ClassDefinition (default) can seem nice and is fine to start.  I have seen it result in updates not being reflected until XCode is restarted.  I am learning to prefer Manual/None. With that I can force generation of files with Editor->Create NSManagedObject Subclass..., have control of those files (if I want to manually edit).

## Custom Extensions
I prefer to put any custom code for my managed objects into an Entity+Customizations.swift file.  Why?  I can regenerate my code with CodeGen if desired, without losing any of my customizations.  Plus it is cleaner to see what is my code vs. what is generated code.  

## Favorite Extensions
Casting NSSet properties as typed Set<T>
```swift
    public var adventurersSet : Set<Adventurer>? {
        return adventurers as? Set<Adventurer>
    }
```
    
For some additional extension ideas see (d)

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


### Simple Fetches

```swift
    /**
     Fetch the singular entity from the container, return nil if not found
     */
    func fetchEntity() -> Engity? {
        let moc = appDelegate!.persistentContainer.viewContext
        let fetchRequest : NSFetchRequest<Entity> = Entity.fetchRequest()
        fetchRequest.resultType = .managedObjectResultType
        fetchRequest.fetchLimit = 1
        do {
            let entities : [Entity] = try moc.fetch(fetchRequest)
            if (entities.count > 0) {
                return entities[0]
            } else {
                return nil
            }
        } catch let error as NSError {
            print("Could not fetch. \(error), \(error.userInfo)")
        }
        return nil
    }
```

## References
 - [a](https://www.codementor.io/@francofantillo/coredata-and-data-persistence-in-ios-uapgfgbxn)
 - [b](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)
 - [c](https://medium.com/@kahseng.lee123/core-data-codegen-explained-462c30341041)
 - [d](https://www.hackingwithswift.com/books/ios-swiftui/one-to-many-relationships-with-core-data-swiftui-and-fetchrequest)
