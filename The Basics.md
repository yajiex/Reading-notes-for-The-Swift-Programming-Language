1. If you need to give a constant or variable the same name as a reserved Swift keyword, surround the keyword with backticks (`) when using it as a name. 
2. To print a value without a line break after it, pass an empty string as the terminator—for example, `print(someValue, terminator: "")`
3. Hexadecimal floats must have an exponent, indicated by an uppercase or lowercase p. `0xFp-2` means 15 x 2-2, or 3.75, `let hexadecimalDouble = 0xC.3p0` means 12.1875
4. Numeric literals can contain extra formatting to make them easier to read. Both integers and floats can be padded with extra zeros and can contain underscores to help with readability. Neither type of formatting affects the underlying value of the literal:

        let paddedDouble = 000123.456
        let oneMillion = 1_000_000
        let justOverOneMillion = 1_000_000.000_000_1

5. tuples
        let http404Error = (404, "Not Found")
        let (statusCode, statusMessage) = http404Error
        let (justTheStatusCode, _) = http404Error
        let http200Status = (statusCode: 200, description: "OK")

6. An implicitly unwrapped optional is a normal optional behind the scenes, but can also be used like a nonoptional value, without the need to unwrap the optional value each time it’s accessed. The following example shows the difference in behavior between an optional string and an implicitly unwrapped optional string when accessing their wrapped value as an explicit String:

        let possibleString: String? = "An optional string."
        let forcedString: String = possibleString! // requires an exclamation mark
        
        let assumedString: String! = "An implicitly unwrapped optional string."
        let implicitString: String = assumedString // no need for an exclamation mark
    If an implicitly unwrapped optional is nil and you try to access its wrapped value, you’ll trigger a runtime error. The result is exactly the same as if you place an exclamation mark after a normal optional that doesn’t contain a value.

    You can still treat an implicitly unwrapped optional like a normal optional, to check if it contains a value:

        if assumedString != nil {
            print(assumedString)
        }
        // Prints "An implicitly unwrapped optional string."

7. The difference between assertions and preconditions is in when they’re checked: Assertions are checked only in debug builds, but preconditions are checked in both debug and production builds. In production builds, the condition inside an assertion isn’t evaluated. 
        // In the implementation of a subscript...
        precondition(index > 0, "Index must be greater than zero.")
    You can also call the `preconditionFailure(_:file:line:)` function to indicate that a failure has occurred—for example, if the default case of a switch was taken, but all valid input data should have been handled by one of the switch’s other cases.

    If you compile in unchecked mode (-Ounchecked), preconditions aren’t checked. The compiler assumes that preconditions are always true, and it optimizes your code accordingly. However, the `fatalError(_:file:line:)` function always halts execution, regardless of optimization settings.

    You can use the `fatalError(_:file:line:)` function during prototyping and early development to create stubs for functionality that hasn’t been implemented yet, by writing `fatalError("Unimplemented")` as the stub implementation. Because fatal errors are never optimized out, unlike assertions or preconditions, you can be sure that execution always halts if it encounters a stub implementation.