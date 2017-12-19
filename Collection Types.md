1. `var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]`
2. Swift’s Set type does not have a defined ordering. To iterate over the values of a set in a specific order, use the sorted() method, which returns the set’s elements as an array sorted using the < operator.

        for genre in favoriteGenres.sorted() {
            print("\(genre)")
        }
        // Classical
        // Hip hop
        // Jazz

3. Unlike a subscript, however, the updateValue(_:forKey:) method returns the old value after performing an update. This enables you to check whether or not an update took place.

The updateValue(_:forKey:) method returns an optional value of the dictionary’s value type. For a dictionary that stores String values, for example, the method returns a value of type String?, or “optional String”. This optional value contains the old value for that key if one existed before the update, or nil if no value existed:

        if let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {
            print("The old value for DUB was \(oldValue).")
        }
        // Prints "The old value for DUB was Dublin."
