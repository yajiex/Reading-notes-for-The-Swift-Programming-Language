1. A multiline string literal includes all of the lines between its opening and closing quotation marks. The string begins on the first line after the opening quotation marks (""") and ends on the line before the closing quotation marks, which means that neither of the strings below start or end with a line break:

        let singleLineString = "These are the same."
        let multilineString = """
        These are the same.
        """
    When your source code includes a line break inside of a multiline string literal, that line break also appears in the string’s value. If you want to use line breaks to make your source code easier to read, but you don’t want the line breaks to be part of the string’s value, write a backslash (\) at the end of those lines:

        let softWrappedQuotation = """
        The White Rabbit put on his spectacles.  "Where shall I begin, \
        please your Majesty?" he asked.
        
        "Begin at the beginning," the King said gravely, "and go on \
        till you come to the end; then stop."
        """
    A multiline string can be indented to match the surrounding code. The whitespace before the closing quotation marks (""") tells Swift what whitespace to ignore before all of the other lines. However, if you write whitespace at the beginning of a line in addition to what’s before the closing quotation marks, that whitespace is included.
2. Swift’s String type is a value type.
3. You can append a Character value to a String variable with the String type’s append() method:

        let exclamationMark: Character = "!"
        welcome.append(exclamationMark)
        // welcome now equals "hello there!"

4. The letter é can be represented as the single Unicode scalar é (LATIN SMALL LETTER E WITH ACUTE, or U+00E9). However, the same letter can also be represented as a pair of scalars—a standard letter e (LATIN SMALL LETTER E, or U+0065), followed by the COMBINING ACUTE ACCENT scalar (U+0301). The COMBINING ACUTE ACCENT scalar is graphically applied to the scalar that precedes it, turning an e into an é when it’s rendered by a Unicode-aware text-rendering system.

In both cases, the letter é is represented as a single Swift Character value that represents an extended grapheme cluster. In the first case, the cluster contains a single scalar; in the second case, it’s a cluster of two scalars:

        let eAcute: Character = "\u{E9}"                         // é
        let combinedEAcute: Character = "\u{65}\u{301}"          // e followed by ́
        // eAcute is é, combinedEAcute is é
    
5. The count of the characters returned by the count property isn’t always the same as the length property of an NSString that contains the same characters. The length of an NSString is based on the number of 16-bit code units within the string’s UTF-16 representation and not the number of Unicode extended grapheme clusters within the string.
6. Unicode Representations of Strings.
        for codeUnit in dogString.utf8 {
            print("\(codeUnit) ", terminator: "")
        }
        print("")
        // Prints "68 111 103 226 128 188 240 159 144 182 "
        for codeUnit in dogString.utf16 {
            print("\(codeUnit) ", terminator: "")
        }
        print("")
        // Prints "68 111 103 8252 55357 56374 "
        for scalar in dogString.unicodeScalars {
            print("\(scalar.value) ", terminator: "")
        }
        print("")
        // Prints "68 111 103 8252 128054 "
        for scalar in dogString.unicodeScalars {
            print("\(scalar) ")
        }
        // D
        // o
        // g
        // ‼
        // 🐶