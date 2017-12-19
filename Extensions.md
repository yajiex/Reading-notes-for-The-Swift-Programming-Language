1. Extensions in Swift can:

    - Add computed instance properties and computed type properties
    - Define instance methods and type methods
    - Provide new initializers
    - Define subscripts
    - Define and use new nested types

            extension Int {
                enum Kind {
                    case negative, zero, positive
                }
                var kind: Kind {
                    switch self {
                    case 0:
                        return .zero
                    case let x where x > 0:
                        return .positive
                    default:
                        return .negative
                    }
                }
            }
    - Make an existing type conform to a protocol

    Notes:

     - Extensions can add new functionality to a type, but they cannot override existing functionality.
     - Extensions can add new computed properties, but they cannot add stored properties, or add property observers to existing properties.
     - Extensions can add new convenience initializers to a class, but they cannot add new designated initializers or deinitializers to a class. Designated initializers and deinitializers must always be provided by the original class implementation. If you use an extension to add an initializer to a value type that provides default values for all of its stored properties and does not define any custom initializers, you can call the default initializer and memberwise initializer for that value type from within your extension’s initializer.