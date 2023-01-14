# coredata-cloudkit

# CloudKit

### Steps

- Data Model should be marked with “Used with Cloudkit”
- All Data Model fields should have default value or optional
- Capability
    - CloudKit
    - Push Notification
    - Background Task ⇒ Remote notifications

### PersistentContainer

```swift
import Foundation
import CoreData
import CloudKit

class CoreDataManager {
    
    let persistentContainer: NSPersistentContainer
    static let shared = CoreDataManager()
    
    var viewContext: NSManagedObjectContext {
        return persistentContainer.viewContext
    }
    
    private init() {
        persistentContainer = NSPersistentCloudKitContainer(name: "TodoAppModel")
        
        // In CloudKit, all object fields should be optional or should have default value
        // CloudKit
        persistentContainer.persistentStoreDescriptions.first!.setOption(true as NSNumber, forKey: NSPersistentHistoryTrackingKey) // History Tracking
        persistentContainer.viewContext.mergePolicy = NSMergeByPropertyStoreTrumpMergePolicy // pick the change from the cloudkit
        persistentContainer.viewContext.automaticallyMergesChangesFromParent = true // also merge background task and main task
        
        persistentContainer.loadPersistentStores { (description, error) in
            if let error = error {
                fatalError("Unable to initialize Core Data \(error)")
            }
        }
    }
    
}
```
