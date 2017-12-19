1. All structures have an automatically-generated memberwise initializer, which you can use to initialize the member properties of new structure instances. Initial values for the properties of the new instance can be passed to the memberwise initializer by name:

        let vga = Resolution(width: 640, height: 480)
    Unlike structures, class instances do not receive a default memberwise initializer. 

2. Structures and Enumerations Are Value Types. Classes Are Reference Types. It can sometimes be useful to find out if two constants or variables refer to exactly the same instance of a class. To enable this, Swift provides two identity operators:

    - Identical to (===)
    - Not identical to (!==)

Use these operators to check whether two constants or variables refer to the same single instance:

    if tenEighty === alsoTenEighty {
        print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
    }
    // Prints "tenEighty and alsoTenEighty refer to the same VideoMode instance."
Note that “identical to” (represented by three equals signs, or ===) does not mean the same thing as “equal to” (represented by two equals signs, or ==):

    - “Identical to” means that two constants or variables of class type refer to exactly the same class instance.
    - “Equal to” means that two instances are considered “equal” or “equivalent” in value, for some appropriate meaning of “equal”, as defined by the type’s designer.

3. In Swift, many basic data types such as String, Array, and Dictionary are implemented as structures. This means that data such as strings, arrays, and dictionaries are copied when they are assigned to a new constant or variable, or when they are passed to a function or method.

This behavior is different from Foundation: NSString, NSArray, and NSDictionary are implemented as classes, not structures. Strings, arrays, and dictionaries in Foundation are always assigned and passed around as a reference to an existing instance, rather than as a copy.

The description above refers to the “copying” of strings, arrays, and dictionaries. The behavior you see in your code will always be as if a copy took place. However, Swift only performs an actual copy behind the scenes when it is absolutely necessary to do so. Swift manages all value copying to ensure optimal performance, and you should not avoid assignment to try to preempt this optimization.