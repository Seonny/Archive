// https://swift.org/documentation/api-design-guidelines/

## “Feel Swifty"

## "Clarity is more important than brevity"

## "APIs have to be read grammatically"

### Generics
   * A number of frameworks like auto layout and core data support fully general generics
```
  let request = NSFetchRequest(entityName: "Animal")
  guard let searchResults = try? context.executeFetchRequest(request) as! [Animal] ...
``` 
   👉
```
  let request : NSFetchRequest<Animal> = Animal.fetchRequest 
  guard let searchResults = try? context.fetch(request) ...
```

### Generic constraints are secondary to function signature

### Warn unused results
```
  @discardableResult // The side effect is first. Return value is a secondary thing.
  func plusOne (_ a: Int) -> Int {
     print(a) // side effect
     return a + 1
  }
  
  plusOne(x) // warning will be occured
  _ = plusOne(x) // In case of just using a side effect
```
  
### Unsafe pointer can't be nil, use optional
  
### Implicitly Unwrapped Optional
  * IUO becomes a strong optional if that will work
  * It is only forced if necessary to type check
  
### Floating Point
  * Provides IEEE-754 properties and operations
  * FloatigPointProtocol
    * Not M_PI but Float.pi, CGFloat.pi

### Whole Module Optimization
  * First compliation will be slow. However it will get faster with caching optimization and compliation

### Omit needless words.
`allViews.removeElement(cancelButton)`

👉  
`allViews.remove(cancelButton)`

### Arguments should show an information that is a relationship to the method.
  * emplyess.remove(somePosition) => emplyess.remove(at: somePosition)
  * mainView.addChild(sideBar, atPoint: origin) => mainView.addChild(sideBar, at: origin)
  * It leads to overload by diffrent type
```
    friends.remove(ted)
    friends.remove(at: positionOfTed)
```    

### First argument should read grammatically
  * Prepositional phrase
  
    `truck.removeBoxes(withLabel: "WWDC 2016")`
  * To make part of a grammatical phrase
  
`viewController.dismiss(true)`
   
👉
`viewController.dismiss(animated: true)`
  * Otherwise, don't use a first argument label

### Name it according to its role.
`var string = "Hello"` 

👉 
`var greeting = "Hello"`
  
  
`func restock(from widgetFactory: WidgetFactory) `
   
👉 
`func restock(from supplier: WidgetFactory)`

### NSObject, Any, AnyObject or a fundamental type could give us weak type information. 
   * To be clarity, A parameter has such a weak type information should have a noun describing its role.
   
`func add(observer: NSObject, for keyPath: String)` 

👉 
`func addObserver(_ observer: NSObject, forKeyPath path: String)`

### Factory method begins its name with "make"

### Initializer and factory method should form a phrase that doesn't include the first argument
   * This rleates to "18.Arguments Labels, Full-width type conversion"
```   
   Color(havingRGBValuesRed: 32...) // Color having RGB values red 32`
```   
👉
```   
   Color(red: 32...) // Color red 32
```   

### Name methods based on their side effects
   * Functions without side effects or have return value should read as noun phrases
    
     `x.distance(to: y), i.successor()`
   * But functions with side effects should read as imperative verb pharese
   
     `friends.reverse()`
   * Mutating, nonmutating
     * Mutating methods have imperative verb forms.
     * Nonmutating methods is applied by “ed”/“ing” rule

ed-rule | ing-rule
--------|---------
`x.reverse() // mutating`|`documentDirectory.appendPathComponent(“.list”) // mutating`
`let y = x.reversed() // non-mutating`|`let documentFile = documentDirectory.appedingPathComponent(“.list) // non-mutating ,To use "appeded", it needs to be changed like "appended to something", so it needs to be applied by ing-rule`

### Use `#selector` & `#keyPath`

### Boolean methods should read as assertions about the receiver
    `x.isEmpty`

### Protocols describe what something is should read as nouns.
    `Collection`

### Protocols describe a capability should be named using the suffixes able, ible or ing.
    `Equatable, ProgressReporting`

### Types, properties, variables and constants should read as nouns.

### About Terms
   * Avoid obscure terms e.g. “epidermis” is not ad good as “skin”
   * Use a term of art as it is.
   * Avoid abbreviations
   * Embrace precedent. 
    * ex) Not verticalPositionsOnUnitCircleAtOriginOfEndOfRadiusWithAngle(x) but sin(x) 

### Document complexity for computed property that is not O(1)

### Names of types and protocols are UpperCamelCase. Everything else is lowerCamelCase. 
   * But Acronyms that appear all upper case in American English should be up- or down-cased according to case conventions. 
    * ex) ASCII(American Standard Code for Information Interchange)
   * Other acronyms should be treated as ordinary words. 
    * ex) Scuba(Self-Contained Underwater Breathing Apparatus)

### Take advantage of defaulted parameters.

```
func compare(other: String) -> Ordering
func compare(other: String, options: CompareOptions) -> Odering  
```

👉 

`func compare(other: String, options: CompareOptions = []) -> Odering`

### Arguments Labels
   * Omit all labels when arguments can not be usefully distinguished.
    * ex) `min(number 1, number 2)`
   * Full-width type conversion: omit the 1st argument label 
    * ex) `Int64(someUInt32)`
   * Narrowing type conversion: describe the narrowing is recommended by label. 
    * ex) `UInt32(truncating: someUInt64)`

### Label closure parameters and tuple members

### Take care with unconstrained polymorphism: Any, AnyObject and unconstrained generic parameters
```    
    var values: [Any] = [1, “a”]
    values.append([2, 3, 4]) // [1, “a”, [2, 3, 4]] or [1, “a”, 2, 3, 4]
```
```
    struct Array {
        	/// Inserts ‘newElement’ at ‘self.endIdnex’
    		public mutating func append(newElement: Element)
        
    		/// Inserts the contents of ‘newElements’, in order, at
    		/// ‘self.endIndex’.
    		public mutating func append<S : SequenceType where S.Generator.Element == Element>(contentsOf newElements: S)
    } 
```  
    `So, [1, “a”, [2, 3, 4]]`


### Use `@objc` to convert a Swifty function to an ObjC method

### New NS_SWIFT_NAME supports argument labeling
```   
   - (NSLayoutConstraint *)constraintEqualToAnchor:(NSLayoutAnchor<AnchorType> *)anchor NS_SWIFT_NAME(constraint(equalTo:));
```   
👉
`func constraint(equalTo: NSLayoutAnchor) -> NSLayoutConstraint`


### NS_EXTENSIBLE_STRING_ENUM
   
```
   typedef NSString * NSCalendarIdentifier NS_EXTENSIBLE_STRING_ENUM;
   
   NSCalendarIdentifier NSClaendarIdentifierGregorian;
```
👉
```
    struct NSCalednarIdentifier : RawRepresentable {
	init(_ rawValue: String)
	var rawValue: String { get }
	static let gregorian: NSCalendarIdentifier
    }

   let cal = NSCalendar(identifier: .gregorian)
```   

### C API
   * Properties
```       
       CFStringRef kCGColorWhite NS_SWIFT_NAME(CGColor.white);
```   
👉
```
       extension CGColor { static let white: CFString }
        
       let color = CGColor.white
```

   * Initializers
```
      CGAffineTransform CGAffineTransformMakeTranslation(CGFloat tx, CGFloat ty) NS_SWIFT_NAME(CGAffineTransform.init(translationX:y:));
```
👉
```      
      extension CGAffineTransform { init(translationX: CGFloat, y: CGFloat) }

      let translate = CGAffineTransform(translationX: 1.0, y: 0.5)
```      

   * Methods
```       
       void CGContextFillPath(CGContextRef) NS_SWIFT_NAME(CGContext.fillPaht(self));
```
👉
```       
       extension CGContext { func fillPath() }

       context.fillPath()
```       

   * Computed Properties
    
```
       CFStringRef ArtistGetName(ArtistRef) NS_SWIFT_NAME(getter:Artist.name(self:));
       void ArtistSetName(ArtistRef, CFStringRef) NS_SWIFT_NAME(setter:Artist.name(self:newValue:));
```
👉
```
       extension Artist { var name: CFString { get set } }
```

```
       let formerName = myArtist.name
        myArtist.name = “Ƭ̵̬̊” // The Artist Formerly Known as Prince
```


### Together
```
    typedef NSString * NSCalendarIdentifier NS_EXTENSIBLE_STRING_ENUM NS_SWIFT_NAME(Calendar.Identifier) ;
    NSCalendarIdentifier NSClaendarIdentifierGregorian;
```
👉
```
    struct Calendar.Identifier : RawRepresentable {
    	init(_ rawValue: String)
       	var rawValue: String { get }
       	static let gregorian: Calendar.Identifier
    }


let cal = NSCalendar(identifier: .gregorian)
```
