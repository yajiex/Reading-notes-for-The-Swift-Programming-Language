1. Unlike C and Objective-C, Swift enumeration cases are not assigned a default integer value when they are created. When you’re working with enumerations that store integer or string raw values, you don’t have to explicitly assign a raw value for each case. When you don’t, Swift will automatically assign the values for you.
2. You can define Swift enumerations to store associated values of any given type, and the value types can be different for each case of the enumeration if needed.
        enum Barcode {
            case upc(Int, Int, Int, Int)
            case qrCode(String)
        }

        switch productBarcode {
        case .upc(let numberSystem, let manufacturer, let product, let check):
            print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
        case .qrCode(let productCode):
            print("QR code: \(productCode).")
        }
        // Prints "QR code: ABCDEFGHIJKLMNOP."

        switch productBarcode {
        case let .upc(numberSystem, manufacturer, product, check):
            print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
        case let .qrCode(productCode):
            print("QR code: \(productCode).")
        }
        // Prints "QR code: ABCDEFGHIJKLMNOP."
    
3. A recursive enumeration is an enumeration that has another instance of the enumeration as the associated value for one or more of the enumeration cases. You indicate that an enumeration case is recursive by writing indirect before it, which tells the compiler to insert the necessary layer of indirection.

For example, here is an enumeration that stores simple arithmetic expressions:

        enum ArithmeticExpression {
            case number(Int)
            indirect case addition(ArithmeticExpression, ArithmeticExpression)
            indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
        }
    You can also write indirect before the beginning of the enumeration to enable indirection for all of the enumeration’s cases that have an associated value:

        indirect enum ArithmeticExpression {
            case number(Int)
            case addition(ArithmeticExpression, ArithmeticExpression)
            case multiplication(ArithmeticExpression, ArithmeticExpression)
        }

        let five = ArithmeticExpression.number(5)
        let four = ArithmeticExpression.number(4)
        let sum = ArithmeticExpression.addition(five, four)
        let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))

        func evaluate(_ expression: ArithmeticExpression) -> Int {
            switch expression {
            case let .number(value):
                return value
            case let .addition(left, right):
                return evaluate(left) + evaluate(right)
            case let .multiplication(left, right):
                return evaluate(left) * evaluate(right)
            }
        }
        
        print(evaluate(product))
        // Prints "18"
