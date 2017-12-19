1. When you access a subscript on an optional value through optional chaining, you place the question mark before the subscript’s brackets, not after. The optional chaining question mark always follows immediately after the part of the expression that is optional.

        if let firstRoomName = john.residence?[0].name {
            print("The first room name is \(firstRoomName).")
        } else {
            print("Unable to retrieve the first room name.")
        }
        // Prints "Unable to retrieve the first room name."