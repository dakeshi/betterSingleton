# Better Singleton Approach in Swift

Use singleton pattern to manage a shared resource, not the global state.

```swift
enum DatabaseType {
  case realm
  case coreData
}

/*:
 ### create the micro framework
 `internal` helps to use in the framework. ex) the test target
 */
class DataManager {

  // Hide singleton pattern from the outside.
  // Do not expose unneccessary information
  static let shared = DataManager()

  let dbType: DatabaseType
  let onlineOnly: Bool

  // Use dependency injection with an default implementation
  // It helps to make easy to test, flexible code
  internal init(dbType: DatabaseType = .realm, onlineOnly: Bool = true){
    self.dbType = dbType
    self.onlineOnly = onlineOnly
  }
}
let dataManager = DataManager()

// create an non-shared instance depending on your scenario
print(dataManager.onlineOnly)
print(DataManager.shared.dbType)


/*:
 ### create an instance of itself
 With `private`, it can not create an instance from the outside.
 */

class Service {
  static let shared = Service()

  private init(){
  //..
  }
}

Service.shared
//let service = Service() // error
```

# reference
### easy to test singleton pattern using dependency injection
- http://www.jessesquires.com/blog/refactoring-singletons-in-swift
### singleton guideline
- http://www.jessesquires.com/blog/writing-better-singletons-in-swift
- https://www.quora.com/Apple-Swift-programming-language-Should-I-choose-shared-instance-singletons-over-static-functions
- https://cocoacasts.com/what-is-a-singleton-and-how-to-create-one-in-swift/
