1. Property requirements are always declared as variable properties, prefixed with the var keyword. Gettable and settable properties are indicated by writing { get set } after their type declaration, and gettable properties are indicated by writing { get }. Always prefix type property requirements with the static keyword when you define them in a protocol. This rule pertains even though type property requirements can be prefixed with the class or static keyword when implemented by a class

        protocol SomeProtocol {
            var mustBeSettable: Int { get set }
            var doesNotNeedToBeSettable: Int { get }
            static var someTypeProperty: Int { get set }
        }

2. If you define a protocol instance method requirement that is intended to mutate instances of any type that adopts the protocol, mark the method with the mutating keyword as part of the protocol’s definition. This enables structures and enumerations to adopt the protocol and satisfy that method requirement.
3. Initializer Requirements.

        protocol SomeProtocol {
            init(someParameter: Int)
        }
        
    You can implement a protocol initializer requirement on a conforming class as either a designated initializer or a convenience initializer. In both cases, you must mark the initializer implementation with the required modifier:

        class SomeClass: SomeProtocol {
            required init(someParameter: Int) {
                // initializer implementation goes here
            }
        }
    
    You don’t need to mark protocol initializer implementations with the required modifier on classes that are marked with the final modifier, because final classes can’t subclassed. 

4. Delegation is a design pattern that enables a class or structure to hand off (or delegate) some of its responsibilities to an instance of another type. This design pattern is implemented by defining a protocol that encapsulates the delegated responsibilities, such that a conforming type (known as a delegate) is guaranteed to provide the functionality that has been delegated. Delegation can be used to respond to a particular action, or to retrieve data from an external source without needing to know the underlying type of that source.
5. You can limit protocol adoption to class types (and not structures or enumerations) by adding the AnyObject protocol to a protocol’s inheritance list.

        protocol SomeClassOnlyProtocol: AnyObject, SomeInheritedProtocol {
            // class-only protocol definition goes here
        }
    In the example above, SomeClassOnlyProtocol can only be adopted by class types. It’s a compile-time error to write a structure or enumeration definition that tries to adopt SomeClassOnlyProtocol. Use a class-only protocol when the behavior defined by that protocol’s requirements assumes or requires that a conforming type has reference semantics rather than value semantics.
6. You can define optional requirements for protocols, These requirements don’t have to be implemented by types that conform to the protocol. Optional requirements are prefixed by the optional modifier as part of the protocol’s definition. Optional requirements are available so that you can write code that interoperates with Objective-C. Both the protocol and the optional requirement must be marked with the @objc attribute. Note that @objc protocols can be adopted only by classes that inherit from Objective-C classes or other @objc classes. They can’t be adopted by structures or enumerations.

        @objc protocol CounterDataSource {
            @objc optional func increment(forCount count: Int) -> Int
            @objc optional var fixedIncrement: Int { get }
        }
    
7. Protocols can be extended to provide method and property implementations to conforming types. This allows you to define behavior on protocols themselves, rather than in each type’s individual conformance or in a global function.

        extension RandomNumberGenerator {
            func randomBool() -> Bool {
                return random() > 0.5
            }
        }

8. When you define a protocol extension, you can specify constraints that conforming types must satisfy before the methods and properties of the extension are available. You write these constraints after the name of the protocol you’re extending using a generic where clause.

    For instance, you can define an extension to the Collection protocol that applies to any collection whose elements conform to the TextRepresentable protocol.
    
        extension Collection where Iterator.Element: TextRepresentable {
            var textualDescription: String {
                let itemsAsText = self.map { $0.textualDescription }
                return "[" + itemsAsText.joined(separator: ", ") + "]"
            }
        }