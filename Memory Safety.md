1. If you have conflicting access to memory from within a single thread, Swift guarantees that you’ll get an error at either compile time or runtime.
2. Conflicting Access to In-Out Parameters.

        var stepSize = 1
        
        func incrementInPlace(_ number: inout Int) {
            number += stepSize
        }
        
        incrementInPlace(&stepSize)
        // Error: conflicting accesses to stepSize
    In the code above, stepSize is a global variable, and it is normally accessible from within incrementInPlace(_:). However, the read access to stepSize overlaps with the write access to number. As shown in the figure below, both number and stepSize refer to the same location in memory. The read and write accesses refer to the same memory and they overlap, producing a conflict.

        func balance(_ x: inout Int, _ y: inout Int) {
            let sum = x + y
            x = sum / 2
            y = sum - x
        }
        var playerOneScore = 42
        var playerTwoScore = 30
        balance(&playerOneScore, &playerTwoScore)  // OK
        balance(&playerOneScore, &playerOneScore)
        // Error: Conflicting accesses to playerOneScore
    The balance(_:_:) function above modifies its two parameters to divide the total value evenly between them. Calling it with playerOneScore and playerTwoScore as arguments doesn’t produce a conflict—there are two write accesses that overlap in time, but they access different locations in memory. In contrast, passing playerOneScore as the value for both parameters produces a conflict because it tries to perform two write accesses to the same location in memory at the same time.

        var playerInformation = (health: 10, energy: 20)
        balance(&playerInformation.health, &playerInformation.energy)
        // Error: conflicting access to properties of playerInformation
    In the example above, calling balance(_:_:) on the elements of a tuple produces a conflict because there are overlapping write accesses to playerInformation. 

    The code below shows that the same error appears for overlapping write accesses to the properties of a structure that’s stored in a global variable.

        var holly = Player(name: "Holly", health: 10, energy: 10)
        balance(&holly.health, &holly.energy)  // Error
    In practice, most access to the properties of a structure can overlap safely. For example, if the variable holly in the example above is changed to a local variable instead of a global variable, the compiler can prove that overlapping access to stored properties of the structure is safe:

        func someFunction() {
            var oscar = Player(name: "Oscar", health: 10, energy: 10)
            balance(&oscar.health, &oscar.energy)  // OK
        }
    In the example above, Oscar’s health and energy are passed as the two in-out parameters to balance(_:_:). The compiler can prove that memory safety is preserved because the two stored properties don’t interact in any way.

    It can prove that overlapping access to properties of a structure is safe if the following conditions apply:

        - You’re accessing only stored properties of an instance, not computed properties or class properties.
        - The structure is the value of a local variable, not a global variable.
        - The structure is either not captured by any closures, or it’s captured only by nonescaping closures.
    If the compiler can’t prove the access is safe, it doesn’t allow the access.