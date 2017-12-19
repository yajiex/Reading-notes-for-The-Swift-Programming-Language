1. Where this is appropriate, you access the superclass version of a method, property, or subscript by using the super prefix:

    - An overridden method named someMethod() can call the superclass version of someMethod() by calling super.someMethod() within the overriding method implementation.
    - An overridden property called someProperty can access the superclass version of someProperty as super.someProperty within the overriding getter or setter implementation.
    - An overridden subscript for someIndex can access the superclass version of the same subscript as super[someIndex] from within the overriding subscript implementation.

2. You can use property overriding to add property observers to an inherited property. This enables you to be notified when the value of an inherited property changes, regardless of how that property was originally implemented.

        class AutomaticCar: Car {
            override var currentSpeed: Double {
                didSet {
                    gear = Int(currentSpeed / 10.0) + 1
                }
            }
        }

3. You can prevent a method, property, or subscript from being overridden by marking it as final. Do this by writing the final modifier before the method, property, or subscript’s introducer keyword (such as final var, final func, final class func, and final subscript).