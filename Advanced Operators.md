1. Unlike arithmetic operators in C, arithmetic operators in Swift do not overflow by default. Overflow behavior is trapped and reported as an error. To opt in to overflow behavior, use Swift’s second set of arithmetic operators that overflow by default, such as the overflow addition operator (&+). All of these overflow operators begin with an ampersand (&).
2. Operator Methods.

        struct Vector2D {
            var x = 0.0, y = 0.0
        }
        
        extension Vector2D {
            static func + (left: Vector2D, right: Vector2D) -> Vector2D {
                return Vector2D(x: left.x + right.x, y: left.y + right.y)
            }
        }
    
    Prefix and Postfix Operators

        extension Vector2D {
            static prefix func - (vector: Vector2D) -> Vector2D {
                return Vector2D(x: -vector.x, y: -vector.y)
            }
        }

        let positive = Vector2D(x: 3.0, y: 4.0)
        let negative = -positive
        // negative is a Vector2D instance with values of (-3.0, -4.0)
        let alsoPositive = -negative
        // alsoPositive is a Vector2D instance with values of (3.0, 4.0)
    
    Compound Assignment Operators

        extension Vector2D {
            static func += (left: inout Vector2D, right: Vector2D) {
                left = left + right
            }
        }

        var original = Vector2D(x: 1.0, y: 2.0)
        let vectorToAdd = Vector2D(x: 3.0, y: 4.0)
        original += vectorToAdd
        // original now has values of (4.0, 6.0)

    It is not possible to overload the default assignment operator (=). Only the compound assignment operators can be overloaded. Similarly, the ternary conditional operator (a ? b : c) cannot be overloaded.

    Equivalence Operators

        extension Vector2D {
            static func == (left: Vector2D, right: Vector2D) -> Bool {
                return (left.x == right.x) && (left.y == right.y)
            }
            static func != (left: Vector2D, right: Vector2D) -> Bool {
                return !(left == right)
            }
        }

        let twoThree = Vector2D(x: 2.0, y: 3.0)
        let anotherTwoThree = Vector2D(x: 2.0, y: 3.0)
        if twoThree == anotherTwoThree {
            print("These two vectors are equivalent.")
        }
        // Prints "These two vectors are equivalent."

    Custom Operators

        extension Vector2D {
            static prefix func +++ (vector: inout Vector2D) -> Vector2D {
                vector += vector
                return vector
            }
        }
        
        var toBeDoubled = Vector2D(x: 1.0, y: 4.0)
        let afterDoubling = +++toBeDoubled
        // toBeDoubled now has values of (2.0, 8.0)
        // afterDoubling also has values of (2.0, 8.0)

    Precedence for Custom Infix Operators

        extension Vector2D {
            static func +- (left: Vector2D, right: Vector2D) -> Vector2D {
                return Vector2D(x: left.x + right.x, y: left.y - right.y)
            }
        }
        let firstVector = Vector2D(x: 1.0, y: 2.0)
        let secondVector = Vector2D(x: 3.0, y: 4.0)
        let plusMinusVector = firstVector +- secondVector
        // plusMinusVector is a Vector2D instance with values of (4.0, -2.0)
    
    You do not specify a precedence when defining a prefix or postfix operator. However, if you apply both a prefix and a postfix operator to the same operand, the postfix operator is applied first.


