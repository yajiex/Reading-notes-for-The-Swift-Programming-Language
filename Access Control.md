1. Swift provides five different access levels for entities within your code. These access levels are relative to the source file in which an entity is defined, and also relative to the module that source file belongs to.

    - Open access and public access enable entities to be used within any source file from their defining module, and also in a source file from another module that imports the defining module. You typically use open or public access when specifying the public interface to a framework. The difference between open and public access is described below.
    - Internal access enables entities to be used within any source file from their defining module, but not in any source file outside of that module. You typically use internal access when defining an app’s or a framework’s internal structure.
    - File-private access restricts the use of an entity to its own defining source file. Use file-private access to hide the implementation details of a specific piece of functionality when those details are used within an entire file.
    - Private access restricts the use of an entity to the enclosing declaration, and to extensions of that declaration that are in the same file. Use private access to hide the implementation details of a specific piece of functionality when those details are used only within a single declaration.

    Open access is the highest (least restrictive) access level and private access is the lowest (most restrictive) access level. Open access applies only to classes and class members, and it differs from public access as follows:

    - Classes with public access, or any more restrictive access level, can be subclassed only within the module where they’re defined.
    - Class members with public access, or any more restrictive access level, can be overridden by subclasses only within the module where they’re defined.
    - Open classes can be subclassed within the module where they’re defined, and within any module that imports the module where they’re defined.
    - Open class members can be overridden by subclasses within the module where they’re defined, and within any module that imports the module where they’re defined.

2. Access levels in Swift follow an overall guiding principle: No entity can be defined in terms of another entity that has a lower (more restrictive) access level. For example:

    - A public variable can’t be defined as having an internal, file-private, or private type, because the type might not be available everywhere that the public variable is used.
    - A function can’t have a higher access level than its parameter types and return type, because the function could be used in situations where its constituent types are unavailable to the surrounding code.

3. A unit test target can access any internal entity, if you mark the import declaration for a product module with the @testable attribute and compile that product module with testing enabled.
4. A public type defaults to having internal members, not public members. If you want a type member to be public, you must explicitly mark it as such. This requirement ensures that the public-facing API for a type is something you opt in to publishing, and avoids presenting the internal workings of a type as public API by mistake.
5. The access level for a tuple type is the most restrictive access level of all types used in that tuple. The access level for a function type is calculated as the most restrictive access level of the function’s parameter types and return type. You must specify the access level explicitly as part of the function’s definition if the function’s calculated access level doesn’t match the contextual default.
6. Nested types defined within a private type have an automatic access level of private. Nested types defined within a file-private type have an automatic access level of file private. Nested types defined within a public type or an internal type have an automatic access level of internal. If you want a nested type within a public type to be publicly available, you must explicitly declare the nested type as public.
7. A subclass can’t have a higher access level than its superclass
8. It’s even valid for a subclass member to call a superclass member that has lower access permissions than the subclass member, as long as the call to the superclass’s member takes place within an allowed access level context (that is, within the same source file as the superclass for a file-private member call, or within the same module as the superclass for an internal member call):

        public class A {
            fileprivate func someMethod() {}
        }
        
        internal class B: A {
            override internal func someMethod() {
                super.someMethod()
            }
        }
    Because superclass A and subclass B are defined in the same source file, it’s valid for the B implementation of someMethod() to call super.someMethod().

9. Custom initializers can be assigned an access level less than or equal to the type that they initialize. The only exception is for required initializers. A required initializer must have the same access level as the class it belongs to. As with function and method parameters, the types of an initializer’s parameters can’t be more private than the initializer’s own access level.

10. The access level of each requirement within a protocol definition is automatically set to the same access level as the protocol. You can’t set a protocol requirement to a different access level than the protocol it supports. This ensures that all of the protocol’s requirements will be visible on any type that adopts the protocol. 

    If you define a public protocol, the protocol’s requirements require a public access level for those requirements when they’re implemented. This behavior is different from other types, where a public type definition implies an access level of internal for the type’s members. 
    
    If you define a new protocol that inherits from an existing protocol, the new protocol can have at most the same access level as the protocol it inherits from.


11. You can mark an extension with an explicit access-level modifier (for example, private extension) to set a new default access level for all members defined within the extension. This new default can still be overridden within the extension for individual type members. You can’t provide an explicit access-level modifier for an extension if you’re using that extension to add protocol conformance. Instead, the protocol’s own access level is used to provide the default access level for each protocol requirement implementation within the extension. Extensions that are in the same file as the class, structure, or enumeration that they extend behave as if the code in the extension had been written as part of the original type’s declaration. 


12. Any type aliases you define are treated as distinct types for the purposes of access control. A type alias can have an access level less than or equal to the access level of the type it aliases. For example, a private type alias can alias a private, file-private, internal, public, or open type, but a public type alias can’t alias an internal, file-private, or private type.





