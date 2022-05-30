# ios-coredata-development-guide

This is an iOS Core Data development guide that I conclude from Apple Document, Apple sample code, and my iOS development experience. I list sample code to demonstrate how to use Core Data to create local database for your iOS app, and how to sync Core Data's viewContext and backgroundContext.

## Reference

Here are Apple's documents that helps me to create this guide, it should contains all Apple's official document, including those document which is archived. If somthing isn't list here, please let me know and I will update it.

- Apple

  - Development Document
    - [What Is Core Data?](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/index.html#//apple_ref/doc/uid/TP40001075-CH2-SW1)
    - [Concurrency](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/Concurrency.html#//apple_ref/doc/uid/TP40001075-CH24-SW1)
    - [Connecting the Model to Views](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/nsfetchedresultscontroller.html#//apple_ref/doc/uid/TP40001075-CH8-SW1)
    - [Managed Objects and References](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/MO_Lifecycle.html#//apple_ref/doc/uid/TP40001075-CH31-SW1)
    - [Performance](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/Performance.html#//apple_ref/doc/uid/TP40001075-CH25-SW1)

  - Article Document
    - [Creating a core data model](https://developer.apple.com/documentation/coredata/creating_a_core_data_model)
    - [Setting up a core data stack](https://developer.apple.com/documentation/coredata/setting_up_a_core_data_stack)
    - [Setting up a core data stack manually](https://developer.apple.com/documentation/coredata/setting_up_a_core_data_stack/setting_up_a_core_data_stack_manually)
    - [Using core data in the background](https://developer.apple.com/documentation/coredata/using_core_data_in_the_background)
    - [Consuming relevant store changes](https://developer.apple.com/documentation/coredata/consuming_relevant_store_changes)
    - [Accessing data when the store changes](https://developer.apple.com/documentation/coredata/accessing_data_when_the_store_changes)

  - Class Document
    - [Core data stack](https://developer.apple.com/documentation/coredata/core_data_stack)
    - [NSManagedObjectContext](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext)
    - [Persistent history](https://developer.apple.com/documentation/coredata/persistent_history)

  - Sample Code
    - [Loading and Displaying a Large Data Feed](https://developer.apple.com/documentation/coredata/loading_and_displaying_a_large_data_feed)

  - Archived Document
    - [Core Data](https://developer.apple.com/library/archive/documentation/DataManagement/Devpedia-CoreData/Introduction.html#//apple_ref/doc/uid/TP40010398-CH33-DontLinkElementID_2)
    - [About Making Batch Changes](https://developer.apple.com/library/archive/featuredarticles/CoreData_Batch_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016719-SW1)
    - [Implementing Batch Updates](https://developer.apple.com/library/archive/featuredarticles/CoreData_Batch_Guide/BatchUpdates/BatchUpdates.html#//apple_ref/doc/uid/TP40016086-CH2-SW1)
    - [Implementing Batch Deletes](https://developer.apple.com/library/archive/featuredarticles/CoreData_Batch_Guide/BatchDeletes/BatchDeletes.html#//apple_ref/doc/uid/TP40016086-CH3-SW1)


## Introduction

This guid has 3 major topic:
- WWDC Core Data sessions brief summary
- Core Data performance tips
- Explains and demonstrate how to sync data between viewContext and backgroundContext in Core Data to achieve concurrency

## WWDC Core Data sessions brief summary since 2020

- 2020: Core Data: Sundries and maxims (https://developer.apple.com/videos/play/wwdc2020/10017/)
  - Mentioned to use NSBatchInsertRequest
  - Mentioned to use NSBatchUpdateRequest
  - Mentioned to use NSBatchDeleteRequest
  - Mentioned to use unique constraints for data UPSERT
  - and more

- 2019: Making Apps with Core Data (https://developer.apple.com/videos/play/wwdc2019/230/)
  - Mentioned to use automaticallyMergesChangesFromParent
  - Introduce NSBatchInsertRequest
  - Introduce NSFetchedResultsController with new Diffable Data Source Snapshot
  - Introduce Fetching Persistent History
  - Core Data unit testing
  - and more

- 2018: Core Data Best Practices (https://developer.apple.com/videos/play/wwdc2018/224/)
  - Mentioned to use NSPersistenContainer
  - Introduce how to use .xcdatamodel outsize main bundle
  - Mentioned to use unique constraints
  - and more

- 2017: What's New in Core Data (https://developer.apple.com/videos/play/wwdc2017/210/)
  - Introduce CoreSpotlight integration with Core Data
  - Introduce Fetch Index Element
  - and more

- 2016: What's New in Core Data (https://developer.apple.com/videos/play/wwdc2016/242/)
  - Introduce NSPersistentStoreDescription
  - Introduce NSPersistentContainer
  - NSFetchRequestResult supports Generics
  - and more

- 2015: What's New in Core Data (https://developer.apple.com/videos/play/wwdc2015/220/)
  - Mention to use mergeChangesFromRemoteContextSave
  - Introduce NSBatchDeleteRequest
  - and more

## Performance Tips

- Use NSBatchInsertRequest, NSBatchUpdateRequest, NSBatchDeleteRequest for performance.
- Make use of a background context to avoid blocking your UI.
- Do not pass NSManagedObject instances between queues.
- Use NSPredicate to only fetch what you need.
- Make use of fetch limits and batch fetch.

**Reference:**
- WWDC sessions
- [Core Data Performance](https://www.avanderlee.com/swift/core-data-performance/)

## viewContext and backgroundContext Syncing

- [Instance Method mergeChanges(fromContextDidSave:) (iOS 3.0+)](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506606-mergechanges)
- [Child-Parent Context relationship (iOS 5.0+)](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/Concurrency.html#//apple_ref/doc/uid/TP40001075-CH24-SW1)
- [Type Method mergeChanges(fromRemoteContextSave:into:) (iOS 9.0+)](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506546-mergechanges)
- [automaticallyMergesChangesFromParent (iOS 10.0+)](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1845237-automaticallymergeschangesfrompa)
- [PersistentHistoryTracking (iOS 11.0+)](https://developer.apple.com/documentation/coredata/consuming_relevant_store_changes)

### Instance Method mergeChanges(fromContextDidSave:) (iOS 3.0+)
```swift
func registerContextUpdateNotification() {
    guard let appDelegate = UIApplication.shared.delegate as? AppDelegate else {
        return
    }

    let backgroundContext = appDelegate.persistentContainer.newBackgroundContext()

    NotificationCenter.default.addObserver(self, selector: #selector(backgroundContextDidChange(_:)), name: Notification.Name.NSManagedObjectContextObjectsDidChange, object: backgroundContext)
    NotificationCenter.default.addObserver(self, selector: #selector(backgroundContextDidChange(_:)), name: Notification.Name.NSManagedObjectContextWillSave, object: backgroundContext)
    NotificationCenter.default.addObserver(self, selector: #selector(backgroundContextDidChange(_:)), name: Notification.Name.NSManagedObjectContextDidSave, object: backgroundContext)

    backgroundContext.perform {
        let person = Person(context: backgroundContext)
        person.name = "name"
        do {
            try backgroundContext.save()
        } catch let error {
            print(error.localizedDescription)
        }
    }
}

@objc
func backgroundContextDidChange(_ notification: Notification) {
    guard let appDelegate = UIApplication.shared.delegate as? AppDelegate else {
        return
    }

    let viewContext = appDelegate.persistentContainer.viewContext

    viewContext.perform {
        viewContext.mergeChanges(fromContextDidSave: notification)
    }
}
```

### Child-Parent Context relationship (iOS 5.0+)

```swift
func syncChildParentContext() {
    let jsonArray = [String]()
    guard let appDelegate = UIApplication.shared.delegate as? AppDelegate else {
        return
    }

    let viewContext = appDelegate.persistentContainer.viewContext

    let privateContext = NSManagedObjectContext(concurrencyType: .privateQueueConcurrencyType)
    privateContext.parent = viewContext

    privateContext.perform {
        for json in jsonArray {
            // Heavy task for updating ManagedObjects with data from the dictionary
        }

        do {
            try privateContext.save()
            privateContext.performAndWait {
                do {
                    try viewContext.save()
                } catch {
                    fatalError("Failure to save context: \(error)")
                }
            }
        } catch {
            fatalError("Failure to save context: \(error)")
        }
    }
}
```

### Type Method mergeChanges(fromRemoteContextSave:into:) (iOS 9.0+)

```swift
func saveData() {
    guard let appDelegate = UIApplication.shared.delegate as? AppDelegate else {
        return
    }

    let viewContext = appDelegate.persistentContainer.viewContext
    let backgroundContext = appDelegate.persistentContainer.newBackgroundContext()

    backgroundContext.perform {
        let changes = try save(
            transformedData: transformedData,
            in: backgroundContext
        )
        NSManagedObjectContext.mergeChanges(
            fromRemoteContextSave: changes,
            into: [viewContext, backgroundContext]
        )
    }
}

func save(
    transformedData: [[String: Any]],
    in context: NSManagedObjectContext
) throws -> [AnyHashable: Any] {
    let insertRequest = NSBatchInsertRequest(
        entity: NSManagedObject.entity(),
        objects: transformedData
    )
    insertRequest.resultType = .objectIDs
    let result = try context.execute(insertRequest) as? NSBatchInsertResult
    return [NSInsertedObjectsKey: result?.result as? [NSManagedObjectID] ?? []]
}
```

### automaticallyMergesChangesFromParent (iOS 10.0+)

```swift
func setupViewContext() {
    guard let appDelegate = UIApplication.shared.delegate as? AppDelegate else {
        return
    }

    let viewContext = appDelegate.persistentContainer.viewContext
    viewContext.automaticallyMergesChangesFromParent = true
}
```

### PersistentHistoryTracking (iOS 11.0+)

- Apple Development Article 
  - [Consuming relevant store changes](https://developer.apple.com/documentation/coredata/consuming_relevant_store_changes)

- Apple Sample Code
  - [Loading and Displaying a Large Data Feed](https://developer.apple.com/documentation/coredata/loading_and_displaying_a_large_data_feed)

## Unit Tests

- Create fake Core Data persistent container

```swift
let persistentContainer: NSPersistentContainer = {
    // URL with path "/dev/null" will create in-memory stores for testing
    // You can refer this by watching WWDC 2018 and 2019 Core Data sessions
    let description = NSPersistentStoreDescription(url: URL(fileURLWithPath: "/dev/null"))

    guard let managedObjectModel = createManagedObjectModel() else {
        fatalError("Failed to create a NSManagedObjectModel")
    }

    let container = NSPersistentContainer(name: coreDataModel.name, managedObjectModel: managedObjectModel)
    container.persistentStoreDescriptions = [description]
    container.loadPersistentStores { description, error in
        if let error = error as NSError? {
            assertionFailure("Unresolved error: \(error), \(error.userInfo)")
        }
    }

    return container
}()

private static func createManagedObjectModel() -> NSManagedObjectModel? {
    let bundle = Bundle.main
    let url = bundle.url(forResource: coreDataModel.name, withExtension: coreDataModel.extension)
    return url.flatMap(NSManagedObjectModel.init)
}
```

## Debug


## Other Reference

- About Concurrency
  - https://ali-akhtar.medium.com/mastering-in-coredata-part-11-multithreading-concurrency-rules-70f1f221dbcd
  - https://ali-akhtar.medium.com/mastering-in-coredata-part-12-multithreading-concurrency-problem-212a85f37930
  - https://ali-akhtar.medium.com/mastering-in-coredata-part-13-multithreading-concurrency-strategy-notifications-63ef0f110293
  - https://ali-akhtar.medium.com/mastering-in-coredata-part-14-multithreading-concurrency-strategy-parent-child-context-305d986f1ac3
  - https://ali-akhtar.medium.com/mastering-in-coredata-part-15-multithreading-concurrency-strategy-parent-child-use-case-1-12a180a6fc34
  - https://ali-akhtar.medium.com/mastering-in-coredata-part-16-multithreading-concurrency-strategy-parent-child-use-case-2-bf7dd7e5245a
  - https://ali-akhtar.medium.com/mastering-in-coredata-part-17-multithreading-concurrency-strategy-context-undomanager-d4a52138978a
  - https://stackoverflow.com/questions/68022996/background-thread-core-data-object-property-changes-doesnt-reflect-on-ui
  - https://uynguyen.github.io/2019/09/01/Best-practice-Core-Data-Concurrency/
  - https://cocoacasts.com/adding-core-data-to-an-existing-swift-project-fetching-data-from-a-persistent-store

- About Performance
  - https://www.avanderlee.com/swift/core-data-performance/
  - https://www.hackingwithswift.com/read/38/10/optimizing-core-data-performance-using-nsfetchedresultscontroller

- About NSKeyedUnarchiveFromData
  - https://www.kairadiagne.com/2020/01/13/nssecurecoding-and-transformable-properties-in-core-data.html
  - https://stackoverflow.com/questions/62589985/nskeyedunarchivefromdata-should-not-be-used-to-for-un-archiving-and-will-be-re
