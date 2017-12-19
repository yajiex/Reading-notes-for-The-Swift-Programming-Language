1. When you assign a default value to a stored property, or set its initial value within an initializer, the value of that property is set directly, without calling any property observers.
2. For class instances, a constant property can be modified during initialization only by the class that introduces it. It cannot be modified by a subclass.
3. Swift provides a default initializer for any structure or class that provides default values for all of its properties and does not provide at least one initializer itself. The default initializer simply creates a new instance with all of its properties set to their default values. Structure types automatically receive a memberwise initializer if they do not define any of their own custom initializers. Unlike a default initializer, the structure receives a memberwise initializer even if it has stored properties that do not have default values.
4. For value types, you use self.init to refer to other initializers from the same value type when writing your own custom initializers. You can call self.init only from within an initializer. Note that if you define a custom initializer for a value type, you will no longer have access to the default initializer (or the memberwise initializer, if it is a structure) for that type. This constraint prevents a situation in which additional essential setup provided in a more complex initializer is accidentally circumvented by someone using one of the automatic initializers. If you want your custom value type to be initializable with the default initializer and memberwise initializer, and also with your own custom initializers, write your custom initializers in an extension rather than as part of the value type’s original implementation.
5. To simplify the relationships between designated and convenience initializers, Swift applies the following three rules for delegation calls between initializers:
    - A designated initializer must call a designated initializer from its immediate superclass.
    - A convenience initializer must call another initializer from the same class.
    - A convenience initializer must ultimately call a designated initializer.

    A simple way to remember this is:
    - Designated initializers must always delegate up.
    - Convenience initializers must always delegate across.

6. Class initialization in Swift is a two-phase process.
    - Phase 1

        - A designated or convenience initializer is called on a class.
        - Memory for a new instance of that class is allocated. The memory is not yet initialized.
        - A designated initializer for that class confirms that all stored properties introduced by that class have a value. The memory for these stored properties is now initialized.
        - The designated initializer hands off to a superclass initializer to perform the same task for its own stored properties.
        - This continues up the class inheritance chain until the top of the chain is reached.
        - Once the top of the chain is reached, and the final class in the chain has ensured that all of its stored properties have a value, the instance’s memory is considered to be fully initialized, and phase 1 is complete.
    - Phase 2

        - Working back down from the top of the chain, each designated initializer in the chain has the option to customize the instance further. Initializers are now able to access self and can modify its properties, call its instance methods, and so on.
        - Finally, any convenience initializers in the chain have the option to customize the instance and to work with self.

7. Unlike subclasses in Objective-C, Swift subclasses do not inherit their superclass initializers by default. You always write the override modifier when overriding a superclass designated initializer, even if your subclass’s implementation of the initializer is a convenience initializer. However, superclass initializers are automatically inherited if certain conditions are met.

    - Rule 1
        - If your subclass doesn’t define any designated initializers, it automatically inherits all of its superclass designated initializers.

    - Rule 2
        - If your subclass provides an implementation of all of its superclass designated initializers—either by inheriting them as per rule 1, or by providing a custom implementation as part of its definition—then it automatically inherits all of the superclass convenience initializers.

8. Failable Initializers.

        struct Animal {
            let species: String
            init?(species: String) {
                if species.isEmpty { return nil }
                self.species = species
            }
        }

9. Write the required modifier before the definition of a class initializer to indicate that every subclass of the class must implement that initializer:

        class SomeClass {
            required init() {
                // initializer implementation goes here
            }
        }
    You must also write the required modifier before every subclass implementation of a required initializer, to indicate that the initializer requirement applies to further subclasses in the chain. You do not write the override modifier when overriding a required designated initializer:

        class SomeSubclass: SomeClass {
            required init() {
                // subclass implementation of the required initializer goes here
            }
        }
    You do not have to provide an explicit implementation of a required initializer if you can satisfy the requirement with an inherited initializer.

10. If a stored property’s default value requires some customization or setup, you can use a closure or global function to provide a customized default value for that property. 

        class SomeClass {
            let someProperty: SomeType = {
                // create a default value for someProperty inside this closure
                // someValue must be of the same type as SomeType
                return someValue
            }()
        }

    Note that the closure’s end curly brace is followed by an empty pair of parentheses. This tells Swift to execute the closure immediately. If you omit these parentheses, you are trying to assign the closure itself to the property, and not the return value of the closure.

    If you use a closure to initialize a property, remember that the rest of the instance has not yet been initialized at the point that the closure is executed. This means that you cannot access any other property values from within your closure, even if those properties have default values. You also cannot use the implicit self property, or call any of the instance’s methods.