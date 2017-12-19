1. The assignment operator in Swift does not itself return a value. The following statement is not valid:

        if x = y {
            // This is not valid, because x = y does not return a value.
        }

2. Swift also provides two identity operators (`===` and `!==`), which you use to test whether two object references both refer to the same object instance.
3. Two tuples of type (String, Bool) can’t be compared with the < operator because the < operator can’t be applied to Bool values.

        ("blue", -1) < ("purple", 1)        // OK, evaluates to true
        ("blue", false) < ("purple", true)  // Error because < can't compare Boolean values

    The Swift standard library includes tuple comparison operators for tuples with fewer than seven elements. To compare tuples with seven or more elements, you must implement the comparison operators yourself.
4. One-Sided Ranges. The closed range operator has an alternative form for ranges that continue as far as possible in one direction—for example, a range that includes all the elements of an array from index 2 to the end of the array. In these cases, you can omit the value from one side of the range operator. This kind of range is called a one-sided range because the operator has a value on only one side. For example:

        for name in names[2...] {
            print(name)
        }
        // Brian
        // Jack
        
        for name in names[...2] {
            print(name)
        }
        // Anna
        // Alex
        // Brian
    The half-open range operator also has a one-sided form that’s written with only its final value. Just like when you include a value on both sides, the final value isn’t part of the range. For example:

        for name in names[..<2] {
            print(name)
        }
        // Anna
        // Alex
    One-sided ranges can be used in other contexts, not just in subscripts. You can’t iterate over a one-sided range that omits a first value, because it isn’t clear where iteration should begin. You can iterate over a one-sided range that omits its final value; however, because the range continues indefinitely, make sure you add an explicit end condition for the loop. You can also check whether a one-sided range contains a particular value, as shown in the code below.

        let range = ...5
        range.contains(7)   // false
        range.contains(4)   // true
        range.contains(-1)  // true

5. The Swift logical operators `&&`and `||` are left-associative, meaning that compound expressions with multiple logical operators evaluate the leftmost subexpression first.