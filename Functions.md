1. You can use a tuple type as the return type for a function to return multiple values as part of one compound return value.

        func minMax(array: [Int]) -> (min: Int, max: Int) {
            var currentMin = array[0]
            var currentMax = array[0]
            for value in array[1..<array.count] {
                if value < currentMin {
                    currentMin = value
                } else if value > currentMax {
                    currentMax = value
                }
            }
            return (currentMin, currentMax)
        }

2. Function Argument Labels and Parameter Names.
        func someFunction(argumentLabel parameterName: Int) {
            // In the function body, parameterName refers to the argument value
            // for that parameter.
        }

        func greet(person: String, from hometown: String) -> String {
            return "Hello \(person)!  Glad you could visit from \(hometown)."
        }
        print(greet(person: "Bill", from: "Cupertino"))
        // Prints "Hello Bill!  Glad you could visit from Cupertino."

3. A variadic parameter accepts zero or more values of a specified type. The values passed to a variadic parameter are made available within the function’s body as an array of the appropriate type.

        func arithmeticMean(_ numbers: Double...) -> Double {
            var total: Double = 0
            for number in numbers {
                total += number
            }
            return total / Double(numbers.count)
        }
        arithmeticMean(1, 2, 3, 4, 5)
        // returns 3.0, which is the arithmetic mean of these five numbers
        arithmeticMean(3, 8.25, 18.75)
        // returns 10.0, which is the arithmetic mean of these three numbers

    A function may have at most one variadic parameter.

4. In-Out parameters.
        func swapTwoInts(_ a: inout Int, _ b: inout Int) {
            let temporaryA = a
            a = b
            b = temporaryA
        }

        var someInt = 3
        var anotherInt = 107
        swapTwoInts(&someInt, &anotherInt)
        print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
        // Prints "someInt is now 107, and anotherInt is now 3"

5. All of the functions you have encountered so far in this chapter have been examples of global functions, which are defined at a global scope. You can also define functions inside the bodies of other functions, known as nested functions. Nested functions are hidden from the outside world by default, but can still be called and used by their enclosing function. An enclosing function can also return one of its nested functions to allow the nested function to be used in another scope.

        func chooseStepFunction(backward: Bool) -> (Int) -> Int {
            func stepForward(input: Int) -> Int { return input + 1 }
            func stepBackward(input: Int) -> Int { return input - 1 }
            return backward ? stepBackward : stepForward
        }
        var currentValue = -4
        let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)
        // moveNearerToZero now refers to the nested stepForward() function
        while currentValue != 0 {
            print("\(currentValue)... ")
            currentValue = moveNearerToZero(currentValue)
        }
        print("zero!")
        // -4...
        // -3...
        // -2...
        // -1...
        // zero!

