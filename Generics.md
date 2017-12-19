1. Generic Types.

        struct Stack<Element> {
            var items = [Element]()
            mutating func push(_ item: Element) {
                items.append(item)
            }
            mutating func pop() -> Element {
                return items.removeLast()
            }
        }

    When you extend a generic type, you don’t provide a type parameter list as part of the extension’s definition. Instead, the type parameter list from the original type definition is available within the body of the extension, and the original type parameter names are used to refer to the type parameters from the original definition.

        extension Stack {
            var topItem: Element? {
                return items.isEmpty ? nil : items[items.count - 1]
            }
        }

2. You write type constraints by placing a single class or protocol constraint after a type parameter’s name, separated by a colon, as part of the type parameter list. The basic syntax for type constraints on a generic function is shown below (although the syntax is the same for generic types):

        func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
            // function body goes here
        }
    
3. When defining a protocol, it’s sometimes useful to declare one or more associated types as part of the protocol’s definition. An associated type gives a placeholder name to a type that is used as part of the protocol. The actual type to use for that associated type isn’t specified until the protocol is adopted. Associated types are specified with the associatedtype keyword.

        protocol Container {
            associatedtype Item
            mutating func append(_ item: Item)
            var count: Int { get }
            subscript(i: Int) -> Item { get }
        }

        struct Stack<Element>: Container {
            // original Stack<Element> implementation
            var items = [Element]()
            mutating func push(_ item: Element) {
                items.append(item)
            }
            mutating func pop() -> Element {
                return items.removeLast()
            }
            // conformance to the Container protocol
            mutating func append(_ item: Element) {
                self.push(item)
            }
            var count: Int {
                return items.count
            }
            subscript(i: Int) -> Element {
                return items[i]
            }
        }

4. You can extend an existing type to add conformance to a protocol, Swift’s Array type already provides an append(_:) method, a count property, and a subscript with an Int index to retrieve its elements. These three capabilities match the requirements of the Container protocol. This means that you can extend Array to conform to the Container protocol simply by declaring that Array adopts the protocol. You do this with an empty extension.

        extension Array: Container {}

5. You can add a type annotation to an associated type in a protocol, to require that conforming types satisfy the constraints described by the type annotation. For example, the following code defines a version of Container that requires the items in the container to be equatable.

        protocol Container {
            associatedtype Item: Equatable
            mutating func append(_ item: Item)
            var count: Int { get }
            subscript(i: Int) -> Item { get }
        }

6. Generic Where Clauses.

        func allItemsMatch<C1: Container, C2: Container>
            (_ someContainer: C1, _ anotherContainer: C2) -> Bool
            where C1.Item == C2.Item, C1.Item: Equatable {
                
                // Check that both containers contain the same number of items.
                if someContainer.count != anotherContainer.count {
                    return false
                }
                
                // Check each pair of items to see if they're equivalent.
                for i in 0..<someContainer.count {
                    if someContainer[i] != anotherContainer[i] {
                        return false
                    }
                }
                
                // All items match, so return true.
                return true
        }
    
    You can include a generic where clause on an associated type.

        protocol Container {
            associatedtype Item
            mutating func append(_ item: Item)
            var count: Int { get }
            subscript(i: Int) -> Item { get }
            
            associatedtype Iterator: IteratorProtocol where Iterator.Element == Item
            func makeIterator() -> Iterator
        }

7. Generic Subscripts. Subscripts can be generic, and they can include generic where clauses. You write the placeholder type name inside angle brackets after subscript, and you write a generic where clause right before the opening curly brace of the subscript’s body. For example:

        extension Container {
            subscript<Indices: Sequence>(indices: Indices) -> [Item]
                where Indices.Iterator.Element == Int {
                    var result = [Item]()
                    for index in indices {
                        result.append(self[index])
                    }
                    return result
            }
        }