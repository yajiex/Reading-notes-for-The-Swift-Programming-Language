1. Closure expression syntax has the following general form:

        { (parameters) -> return type in
            statements
        }

        reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
            return s1 > s2
        })

    The sorted(by:) method is being called on an array of strings, so its argument must be a function of type (String, String) -> Bool. This means that the (String, String) and Bool types do not need to be written as part of the closure expression’s definition. Because all of the types can be inferred, the return arrow (->) and the parentheses around the names of the parameters can also be omitted:

        reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )

    Single-expression closures can implicitly return the result of their single expression by omitting the return keyword from their declaration, as in this version of the previous example:

        reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
    
    If you use these shorthand argument names within your closure expression, you can omit the closure’s argument list from its definition, and the number and type of the shorthand argument names will be inferred from the expected function type. The in keyword can also be omitted, because the closure expression is made up entirely of its body:

        reversedNames = names.sorted(by: { $0 > $1 } )
    
    There’s actually an even shorter way to write the closure expression above. Swift’s String type defines its string-specific implementation of the greater-than operator (>) as a method that has two parameters of type String, and returns a value of type Bool. This exactly matches the method type needed by the sorted(by:) method. 

        reversedNames = names.sorted(by: >)
    
    If you need to pass a closure expression to a function as the function’s final argument and the closure expression is long, it can be useful to write it as a trailing closure instead.

        reversedNames = names.sorted { $0 > $1 }

2. Functions and closures are reference types. Whenever you assign a function or a closure to a constant or a variable, you are actually setting that constant or variable to be a reference to the function or closure.
3. A closure is said to escape a function when the closure is passed as an argument to the function, but is called after the function returns. When you declare a function that takes a closure as one of its parameters, you can write @escaping before the parameter’s type to indicate that the closure is allowed to escape.
    One way that a closure can escape is by being stored in a variable that is defined outside the function. As an example, many functions that start an asynchronous operation take a closure argument as a completion handler. The function returns after it starts the operation, but the closure isn’t called until the operation is completed—the closure needs to escape, to be called later. For example:

        var completionHandlers: [() -> Void] = []
        func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
            completionHandlers.append(completionHandler)
        }

4. An autoclosure is a closure that is automatically created to wrap an expression that’s being passed as an argument to a function. It doesn’t take any arguments, and when it’s called, it returns the value of the expression that’s wrapped inside of it. This syntactic convenience lets you omit braces around a function’s parameter by writing a normal expression instead of an explicit closure.

        // customersInLine is ["Ewa", "Barry", "Daniella"]
        func serve(customer customerProvider: @autoclosure () -> String) {
            print("Now serving \(customerProvider())!")
        }
        serve(customer: customersInLine.remove(at: 0))
        // Prints "Now serving Ewa!"