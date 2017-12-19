1. Swift 4: Extensions to a type in the same file can access that type’s private members.
2. Values are never implicitly converted to another type. If you need to convert a value to a different type, explicitly make an instance of the desired type.

        let label = "The width is "
        let width = 94
        let widthLabel = label + String(width)

3. Switches support any kind of data and a wide variety of comparison operations—they aren’t limited to integers and tests for equality.

        let vegetable = "red pepper"
        switch vegetable {
        case "celery":
            print("Add some raisins and make ants on a log.")
        case "cucumber", "watercress":
            print("That would make a good tea sandwich.")
        case let x where x.hasSuffix("pepper"):
            print("Is it a spicy \(x)?")
        default:
            print("Everything tastes good in soup.")
        }

4. Use `..<` to make a range that omits its upper value, and use `...` to make a range that includes both values.
5. Use a tuple to make a compound value—for example, to return multiple values from a function. The elements of a tuple can be referred to either by name or by number.

        func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
            var min = scores[0]
            var max = scores[0]
            var sum = 0
            
            for score in scores {
                if score > max {
                    max = score
                } else if score < min {
                    min = score
                }
                sum += score
            }
            
            return (min, max, sum)
        }
        let statistics = calculateStatistics(scores: [5, 3, 100, 3, 9])
        print(statistics.sum)
        print(statistics.2)

6. Functions can be nested.

        func returnFifteen() -> Int {
            var y = 10
            func add() {
                y += 5
            }
            add()
            return y
        }
        returnFifteen()
Functions are a first-class type. This means that a function can return another function as its value.

        func makeIncrementer() -> ((Int) -> Int) {
            func addOne(number: Int) -> Int {
                return 1 + number
            }
            return addOne
        }
        var increment = makeIncrementer()
        increment(7)
A function can take another function as one of its arguments.

        func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
            for item in list {
                if condition(item) {
                    return true
                }
            }
            return false
        }
        func lessThanTen(number: Int) -> Bool {
            return number < 10
        }
        var numbers = [20, 19, 7, 12]
        hasAnyMatches(list: numbers, condition: lessThanTen)

7. enums
        enum ServerResponse {
            case result(String, String)
            case failure(String)
        }
        
        let success = ServerResponse.result("6:00 am", "8:09 pm")
        let failure = ServerResponse.failure("Out of cheese.")
        
        switch success {
        case let .result(sunrise, sunset):
            print("Sunrise is at \(sunrise) and sunset is at \(sunset).")
        case let .failure(message):
            print("Failure...  \(message)")
        }

8. One of the most important differences between structures and classes is that structures are always copied when they are passed around in your code, but classes are passed by reference.

9. Notice the use of the mutating keyword in the declaration of SimpleStructure to mark a method that modifies the structure. The declaration of SimpleClass doesn’t need any of its methods marked as mutating because methods on a class can always modify the class.

        protocol ExampleProtocol {
            var simpleDescription: String { get }
            mutating func adjust()
        }

        class SimpleClass: ExampleProtocol {
            var simpleDescription: String = "A very simple class."
            var anotherProperty: Int = 69105
            func adjust() {
                simpleDescription += "  Now 100% adjusted."
            }
        }
        var a = SimpleClass()
        a.adjust()
        let aDescription = a.simpleDescription
        
        struct SimpleStructure: ExampleProtocol {
            var simpleDescription: String = "A simple structure"
            mutating func adjust() {
                simpleDescription += " (adjusted)"
            }
        }
        var b = SimpleStructure()
        b.adjust()
        let bDescription = b.simpleDescription

10. Use throw to throw an error and throws to mark a function that can throw an error. If you throw an error in a function, the function returns immediately and the code that called the function handles the error.

        func send(job: Int, toPrinter printerName: String) throws -> String {
            if printerName == "Never Has Toner" {
                throw PrinterError.noToner
            }
            return "Job sent"
        }
There are several ways to handle errors. One way is to use do-catch. Inside the do block, you mark code that can throw an error by writing try in front of it. Inside the catch block, the error is automatically given the name error unless you give it a different name.

        do {
            let printerResponse = try send(job: 1040, toPrinter: "Bi Sheng")
            print(printerResponse)
        } catch {
            print(error)
        }
You can provide multiple catch blocks that handle specific errors. You write a pattern after catch just as you do after case in a switch.

        do {
            let printerResponse = try send(job: 1440, toPrinter: "Gutenberg")
            print(printerResponse)
        } catch PrinterError.onFire {
            print("I'll just put this over here, with the rest of the fire.")
        } catch let printerError as PrinterError {
            print("Printer error: \(printerError).")
        } catch {
            print(error)
        }
Another way to handle errors is to use try? to convert the result to an optional. If the function throws an error, the specific error is discarded and the result is nil. Otherwise, the result is an optional containing the value that the function returned.

        let printerSuccess = try? send(job: 1884, toPrinter: "Mergenthaler")
        let printerFailure = try? send(job: 1885, toPrinter: "Never Has Toner")
Use defer to write a block of code that is executed after all other code in the function, just before the function returns. The code is executed regardless of whether the function throws an error. You can use defer to write setup and cleanup code next to each other, even though they need to be executed at different times.

        var fridgeIsOpen = false
        let fridgeContent = ["milk", "eggs", "leftovers"]
        
        func fridgeContains(_ food: String) -> Bool {
            fridgeIsOpen = true
            defer {
                fridgeIsOpen = false
            }
            
            let result = fridgeContent.contains(food)
            return result
        }
        fridgeContains("banana")
        print(fridgeIsOpen)

11. Use where right before the body to specify a list of requirements—for example, to require the type to implement a protocol, to require two types to be the same, or to require a class to have a particular superclass.

        func anyCommonElements<T: Sequence, U: Sequence>(_ lhs: T, _ rhs: U) -> Bool
            where T.Iterator.Element: Equatable, T.Iterator.Element == U.Iterator.Element {
                for lhsItem in lhs {
                    for rhsItem in rhs {
                        if lhsItem == rhsItem {
                            return true
                        }
                    }
                }
                return false
        }
        anyCommonElements([1, 2, 3], [3])
    Writing <T: Equatable> is the same as writing <T> ... where T: Equatable.