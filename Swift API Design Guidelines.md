// https://swift.org/documentation/api-design-guidelines/

“Feel Swifty"

"Clarity is more important than brevity"

"APIs have to be read grammatically"

1. Omit needless words.
  
  allViews.removeElement(cancelButton) => allViews.remove(cancelButton)

2. Arguments should show an information that is a relationship to the method.
  
  emplyess.remove(somePosition) => emplyess.remove(at: somePosition)
  
  mainView.addChild(sideBar, atPoint: origin) => mainView.addChild(sideBar, at: origin)
  
  - It leads to overload by diffrent type
  
    friends.remove(ted)
  
    friends.remove(at: positionOfTed)

3. First argument should read grammatically
  - Prepositional phrase
    truck.removeBoxes(withLabel: "WWDC 2016")
  - To make part of a grammatical phrase
    viewController.dismiss(true) => viewController.dismiss(animated: true)
  - Otherwise, don't use a first argument label

4. Name it according to its role.
  
   var string = "Hello" => var greeting = "Hello"
  
   func restock(from widgetFactory: WidgetFactory) => func restock(from supplier: WidgetFactory)

5. NSObject, Any, AnyObject or a fundamental type could give us weak type information.
   To be clarity, A parameter has such a weak type information should have a noun describing its role.
  
   func add(observer: NSObject, for keyPath: String) => func addObserver(_ observer: NSObject, forKeyPath path: String)

6. Factory method begins its name with "make"

7. Initializer and factory method should form a phrase that doesn't include the first argument
   
   Color(havingRGBValuesRed: 32...): Color having RGB values red 32 

   => 
   
   Color(red: 32...): Colro red 32

8. Name methods based on their side effects
   - Functions without side effects or have return value should read as noun phrases
     x.distance(to: y), i.successor()
   - But functions with side effects should read as imperative verb pharese
     friends.reverse()
   - Mutating, nonmutating
     Mutating methods have imperative verb forms.
     Nonmutating methods is applied by “ed”/“ing” rule

     
     “ed” rule
     
        x.reverse() // mutating
     
        let y = x.reversed() // non-mutating

     
     “ing” rule: if it can’t be applied by “ed” rule
     
        documentDirectory.appednPathComponent(“.list”) // mutating
      
        let documentFile = documentDirectory.appedingPathComponent(“.list) // non-mutating

9. Use #selector & #keyPath

10. Boolean methods should read as assertions about the receiver
    
    x.isEmpty

11. Protocols describe what something is should read as nouns.
    
    Collection

12. Protocols describe a capability should be named using the suffixes able, ible or ing.
    
    Equatable, ProgressReporting

13. types, properties, variables and constants should read as nouns.

14.  About Terms
     - Avoid obscure terms e.g. “epidermis” is not ad good as “skin”
     - Use a term of art as it is.
     - Avoid abbreviations
     - Embrace precedent. e.g. not verticalPositionsOnUnitCircleAtOriginOfEndOfRadiusWithAngle(x) but sin(x) 

15. Document complexity for computed property that is not O(1)

16. Names of types and protocols are UpperCamelCase. Everything els is lowerCamelCase. 
    
    But Acronyms that appear all upper case in American English should be up- or down-cased according to case conventions. 
    e.g. ASCII(American Standard Code for Information Interchange)
     
    Other acronyms should be treated as ordinary words. 

    e.g. Scuba(Self-Contained Underwater Breathing Apparatus)

17. Take advantage of defaulted parameters.
     
    e.g. func compare(other: String) -> Ordering
        
         func compare(other: String, options: CompareOptions) -> Odering  
	
         => 

         func compare(other: String, options: CompareOptions = []) -> Odering

18.  Arguments Labels
       - Omit all labels when arguments can not be usefully distinguished.
          e.g. min(number 1, number 2)
       - Full-width type conversion: omit the 1st argument label e.g. Int64(someUInt32)
         narrowing type conversion: describe the narrowing is recommended by label. e.g. UInt32(truncating: someUInt64)

19. Label closure parameters and tuple members

20. Take care with unconstrained polymorphism: Anu,AnyObject and unconstrained generic parameters
      
    var values: [Any] = [1, “a”]
     
    values.append([2, 3, 4]) // [1, “a”, [2, 3, 4]] or [1, “a”, 2, 3, 4]
      
    struct Array {
        /// Inserts ‘newElement’ at ‘self.endIdnex’
        public mutating func append(newElement: Element)
              
        /// Inserts the contents of ‘newElements’, in order, at
        /// ‘self.endIndex’.
        public mutating func append<S : SequenceType where S.Generator.Element == Element>(contentsOf newElements: S)
      }
     
       ——> 

      So, [1, “a”, 2, 3, 4]

“Renaming”
1. Use @objc to convert a Swifty function to an ObjC method

2. New NS_SWIFT_NAME supports argument labeling
  
   - (NSLayoutConstraint *)constraintEqualToAnchor:(NSLayoutAnchor<AnchorType> *)anchor NS_SWIFT_NAME(constraint(equalTo:));
  
  => 
  
  func constraint(equalTo: NSLayoutAnchor) -> NSLayoutConstraint

3. NS_EXTENSIBLE_STRING_ENUM
   
   typedef NSString * NSCalendarIdentifier NS_EXTENSIBLE_STRING_ENUM;
   
   NSCalendarIdentifier NSClaendarIdentifierGregorian;
   
   =>
    
   struct NSCalednarIdentifier : RawRepresentable {
    	init(_ rawValue: String)
        var rawValue: String { get }
        static let gregorian: NSCalendarIdentifier
   }
   
   e.g. let cal = NSCalendar(identifier: .gregorian)

4. C API
    - Properties
       
       CFStringRef kCGColorWhite NS_SWIFT_NAME(CGColor.white);
       
       => 

       extension CGColor { static let white: CFString }

       e.g. let color = CGColor.white

    - Initializers
      
      CGAffineTransform CGAffineTransformMakeTranslation(CGFloat tx, CGFloat ty) NS_SWIFT_NAME(CGAffineTransform.init(translationX:y:));
      
      => 
      
      extension CGAffineTransform { init(translationX: CGFloat, y: CGFloat) }

      e.g. let translate = CGAffineTransform(translationX: 1.0, y: 0.5)

    - Methods
       
       void CGContextFillPath(CGContextRef) NS_SWIFT_NAME(CGContext.fillPaht(self));
       
       => 
       
       extension CGContext { func fillPath() }

       e.g. context.fillPath()

    - Computed Properties
       
       CFStringRef ArtistGetName(ArtistRef) NS_SWIFT_NAME(getter:Artist.name(self:));
       
       void ArtistSetName(ArtistRef, CFStringRef) NS_SWIFT_NAME(setter:Artist.name(self:newValue:));
       
       => 
       
       extension Artist { var name: CSTring { get set } }

       e.g. let formerName = myArtist.name
             myArtist.name = “Ƭ̵̬̊” // The Artist Formerly Known as Prince

5. Together
    
    typedef NSString * NSCalendarIdentifier NS_EXTENSIBLE_STRING_ENUM NS_SWIFT_NAME(Calendar.Identifier) ;
    
    NSCalendarIdentifier NSClaendarIdentifierGregorian;
    
    =>

    struct Calendar.Identifier : RawRepresentable {
    	init(_ rawValue: String)
        var rawValue: String { get }
        static let gregorian: Calendar.Identifier
    }
   
     e.g. let cal = NSCalendar(identifier: .gregorian)
