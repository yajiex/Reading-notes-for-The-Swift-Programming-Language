1. Every instance of a type has an implicit property called self, which is exactly equivalent to the instance itself. You use the self property to refer to the current instance within its own instance methods.
2. Structures and enumerations are value types. By default, the properties of a value type cannot be modified from within its instance methods. However, if you need to modify the properties of your structure or enumeration within a particular method, you can opt in to mutating behavior for that method.

        struct Point {
            var x = 0.0, y = 0.0
            mutating func moveBy(x deltaX: Double, y deltaY: Double) {
                x += deltaX
                y += deltaY
            }
        }
        var somePoint = Point(x: 1.0, y: 1.0)
        somePoint.moveBy(x: 2.0, y: 3.0)
        print("The point is now at (\(somePoint.x), \(somePoint.y))")
        // Prints "The point is now at (3.0, 4.0)"
    
    Mutating methods can assign an entirely new instance to the implicit self property. The Point example shown above could have been written in the following way instead:

        struct Point {
            var x = 0.0, y = 0.0
            mutating func moveBy(x deltaX: Double, y deltaY: Double) {
                self = Point(x: x + deltaX, y: y + deltaY)
            }
        }

3. Within the body of a type method, the implicit self property refers to the type itself, rather than an instance of that type. This means that you can use self to disambiguate between type properties and type method parameters, just as you do for instance properties and instance method parameters.
4. Function is marked with the @discardableResult attribute to indicate it’s not necessarily a mistake for code that calls the method to ignore the return value

        @discardableResult
        mutating func advance(to level: Int) -> Bool {
            if LevelTracker.isUnlocked(level) {
                currentLevel = level
                return true
            } else {
                return false
            }
        }