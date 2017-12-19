1. Use the stride(from:to:by:) function to skip the unwanted marks.

        let minuteInterval = 5
        for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
            // render the tick mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 55)
        }
Closed ranges are also available, by using stride(from:through:by:) instead:

        let hours = 12
        let hourInterval = 3
        for tickMark in stride(from: 3, through: hours, by: hourInterval) {
            // render the tick mark every 3 hours (3, 6, 9, 12)
        }

2. The repeat-while loop in Swift is analogous to a do-while loop in other languages.

    Here’s the general form of a repeat-while loop:

        repeat {
            statements
        } while condition

3. Values in switch cases can be checked for their inclusion in an interval. This example uses number intervals to provide a natural-language count for numbers of any size:

        let approximateCount = 62
        let countedThings = "moons orbiting Saturn"
        let naturalCount: String
        switch approximateCount {
        case 0:
            naturalCount = "no"
        case 1..<5:
            naturalCount = "a few"
        case 5..<12:
            naturalCount = "several"
        case 12..<100:
            naturalCount = "dozens of"
        case 100..<1000:
            naturalCount = "hundreds of"
        default:
            naturalCount = "many"
        }
        print("There are \(naturalCount) \(countedThings).")
        // Prints "There are dozens of moons orbiting Saturn."

4. You can use tuples to test multiple values in the same switch statement.

        let somePoint = (1, 1)
        switch somePoint {
        case (0, 0):
            print("\(somePoint) is at the origin")
        case (_, 0):
            print("\(somePoint) is on the x-axis")
        case (0, _):
            print("\(somePoint) is on the y-axis")
        case (-2...2, -2...2):
            print("\(somePoint) is inside the box")
        default:
            print("\(somePoint) is outside of the box")
        }
        // Prints "(1, 1) is inside the box"

5. A switch case can name the value or values it matches to temporary constants or variables, for use in the body of the case. This behavior is known as value binding, because the values are bound to temporary constants or variables within the case’s body.

        let anotherPoint = (2, 0)
        switch anotherPoint {
        case (let x, 0):
            print("on the x-axis with an x value of \(x)")
        case (0, let y):
            print("on the y-axis with a y value of \(y)")
        case let (x, y):
            print("somewhere else at (\(x), \(y))")
        }
        // Prints "on the x-axis with an x value of 2"

6. A switch case can use a where clause to check for additional conditions.

        let yetAnotherPoint = (1, -1)
        switch yetAnotherPoint {
        case let (x, y) where x == y:
            print("(\(x), \(y)) is on the line x == y")
        case let (x, y) where x == -y:
            print("(\(x), \(y)) is on the line x == -y")
        case let (x, y):
            print("(\(x), \(y)) is just some arbitrary point")
        }
        // Prints "(1, -1) is on the line x == -y"

7. If you need C-style fallthrough behavior, you can opt in to this behavior on a case-by-case basis with the fallthrough keyword. The example below uses fallthrough to create a textual description of a number.

        let integerToDescribe = 5
        var description = "The number \(integerToDescribe) is"
        switch integerToDescribe {
        case 2, 3, 5, 7, 11, 13, 17, 19:
            description += " a prime number, and also"
            fallthrough
        default:
            description += " an integer."
        }
        print(description)
        // Prints "The number 5 is a prime number, and also an integer."

    The fallthrough keyword does not check the case conditions for the switch case that it causes execution to fall into. The fallthrough keyword simply causes code execution to move directly to the statements inside the next case (or default case) block, as in C’s standard switch statement behavior.

8. A labeled statement is indicated by placing a label on the same line as the statement’s introducer keyword, followed by a colon.

        label name: while condition {
            statements
        }

        gameLoop: while square != finalSquare {
            diceRoll += 1
            if diceRoll == 7 { diceRoll = 1 }
            switch square + diceRoll {
            case finalSquare:
                // diceRoll will move us to the final square, so the game is over
                break gameLoop
            case let newSquare where newSquare > finalSquare:
                // diceRoll will move us beyond the final square, so roll again
                continue gameLoop
            default:
                // this is a valid move, so find out its effect
                square += diceRoll
                square += board[square]
            }
        }
        print("Game over!")

9. Checking API Availability.

        if #available(platform name version, ..., *) {
            statements to execute if the APIs are available
        } else {
            fallback statements to execute if the APIs are unavailable
        }

