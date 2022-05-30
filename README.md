# ios-coredata-development-guide

This is an iOS Core Data development guide that I conclude from Apple Document, Apple sample code, and my iOS development experience. I list some sample code to demonstrate how to use Core Data to create local database for your iOS app.

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

- Use NSBatchInsertRequest, NSBatchUpdateRequest, NSBatchDeleteRequest for performance
- Make use of a background context to avoid blocking your UI
- Do not pass NSManagedObject instances between queues
- Use NSPredicate to only fetch what you need
- Make use of fetch limits and batch fetch

**Reference:**
- WWDC sessions
- [Core Data Performance](https://www.avanderlee.com/swift/core-data-performance/)

## viewContext and backgroundContext Syncing






































