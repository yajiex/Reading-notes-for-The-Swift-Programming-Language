1. Error handling in Swift resembles exception handling in other languages, with the use of the try, catch and throw keywords. Unlike exception handling in many languages—including Objective-C—error handling in Swift does not involve unwinding the call stack, a process that can be computationally expensive. As such, the performance characteristics of a throw statement are comparable to those of a return statement.
2. A function marked with throws is called a throwing function. If the function specifies a return type, you write the throws keyword before the return arrow (->).

        func canThrowErrors() throws -> String

3. Throwing initializers can propagate errors in the same way as throwing functions. For example, the initializer for the PurchasedSnack structure in the listing below calls a throwing function as part of the initialization process, and it handles any errors that it encounters by propagating them to its caller.

        struct PurchasedSnack {
            let name: String
            init(name: String, vendingMachine: VendingMachine) throws {
                try vendingMachine.vend(itemNamed: name)
                self.name = name
            }
        }

4. Here is the general form of a do-catch statement:

        do {
            try expression
            statements
        } catch pattern 1 {
            statements
        } catch pattern 2 where condition {
            statements
        }

        var vendingMachine = VendingMachine()
        vendingMachine.coinsDeposited = 8
        do {
            try buyFavoriteSnack(person: "Alice", vendingMachine: vendingMachine)
        } catch VendingMachineError.invalidSelection {
            print("Invalid Selection.")
        } catch VendingMachineError.outOfStock {
            print("Out of Stock.")
        } catch VendingMachineError.insufficientFunds(let coinsNeeded) {
            print("Insufficient funds. Please insert an additional \(coinsNeeded) coins.")
        }
        // Prints "Insufficient funds. Please insert an additional 2 coins."

5. Sometimes you know a throwing function or method won’t, in fact, throw an error at runtime. On those occasions, you can write try! before the expression to disable error propagation and wrap the call in a runtime assertion that no error will be thrown. If an error actually is thrown, you’ll get a runtime error.

    For example, the following code uses a loadImage(atPath:) function, which loads the image resource at a given path or throws an error if the image can’t be loaded. In this case, because the image is shipped with the application, no error will be thrown at runtime, so it is appropriate to disable error propagation.

        let photo = try! loadImage(atPath: "./Resources/John Appleseed.jpg")

6. You use a defer statement to execute a set of statements just before code execution leaves the current block of code. Deferred actions are executed in the reverse of the order that they’re written in your source code. That is, the code in the first defer statement executes last, the code in the second defer statement executes second to last, and so on. The last defer statement in source code order executes first.

        func processFile(filename: String) throws {
            if exists(filename) {
                let file = open(filename)
                defer {
                    close(file)
                }
                while let line = try file.readline() {
                    // Work with the file.
                }
                // close(file) is called here, at the end of the scope.
            }
        }