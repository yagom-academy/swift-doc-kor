
# Conventions

## General Conventions

- <i><span style="color: #C0C0C0">**Document the complexity of any computed property that is not O(1).** People often assume that property access involves no significant computation, because they have stored properties as a mental model. Be sure to alert them when that assumption may be violated.</span></i>

- <i><span style="color: #C0C0C0">**Prefer methods and properties to free functions.** Free functions are used only in special cases:</span></i>

    1. <i><span style="color: #C0C0C0">When there’s no obvious self:</span></i>
        ```swift
        min(x, y, z)
        ```

    2. <i><span style="color: #C0C0C0">When the function is an unconstrained generic:</span></i>
        ```swift
        print(x)
        ```

    3. <i><span style="color: #C0C0C0">When function syntax is part of the established domain notation:</span></i>
        ```swift
        sin(x)
        ```

- <i><span style="color: #C0C0C0">**Follow case conventions.** Names of types and protocols are UpperCamelCase. Everything else is lowerCamelCase.</span></i>

    <i><span style="color: #C0C0C0">[Acronyms and initialisms](https://en.wikipedia.org/wiki/Acronym) that commonly appear as all upper case in American English should be uniformly up- or down-cased according to case conventions:</span></i>

    ```swift
    var utf8Bytes: [UTF8.CodeUnit]
    var isRepresentableAsASCII = true
    var userSMTPServer: SecureSMTPServer
    ```

    <i><span style="color: #C0C0C0">Other acronyms should be treated as ordinary words:</span></i>

    ```swift
    var radarDetector: RadarScanner
    var enjoysScubaDiving = true
    ```

- <i><span style="color: #C0C0C0">**Methods can share a base name** when they share the same basic meaning or when they operate in distinct domains.</span></i>

    <i><span style="color: #C0C0C0">For example, the following is encouraged, since the methods do essentially the same things:</span></i>

    ```swift
    ✅
    extension Shape {
        /// Returns `true` iff `other` is within the area of `self`.
        func contains(_ other: Point) -> Bool { ... }

        /// Returns `true` iff `other` is entirely within the area of `self`.
        func contains(_ other: Shape) -> Bool { ... }

        /// Returns `true` iff `other` is within the area of `self`.
        func contains(_ other: LineSegment) -> Bool { ... }
    }
    ```

    <i><span style="color: #C0C0C0">And since geometric types and collections are separate domains, this is also fine in the same program:</span></i>

    ```swift
    ✅
    extension Collection where Element : Equatable {
        /// Returns `true` iff `self` contains an element equal to
        /// `sought`.
        func contains(_ sought: Element) -> Bool { ... }
    }
    ```

    <i><span style="color: #C0C0C0">However, these index methods have different semantics, and should have been named differently:</span></i>

    ```swift
    ❌
    extension Database {
        /// Rebuilds the database's search index
        func index() { ... }

        /// Returns the `n`th row in the given table.
        func index(_ n: Int, inTable: TableID) -> TableRow { ... }
    }
    ```

    <i><span style="color: #C0C0C0">Lastly, avoid “overloading on return type” because it causes ambiguities in the presence of type inference.</span></i>

    ```swift
    ❌
    extension Box {
        /// Returns the `Int` stored in `self`, if any, and
        /// `nil` otherwise.
        func value() -> Int? { ... }

        /// Returns the `String` stored in `self`, if any, and
        /// `nil` otherwise.
        func value() -> String? { ... }
    }
    ```

<br>

## Parameters

```swift
func move(from start: Point, to end: Point)
```

- <i><span style="color: #C0C0C0">**Choose parameter names to serve documentation.** Even though parameter names do not appear at a function or method’s point of use, they play an important explanatory role.</span></i>

    <i><span style="color: #C0C0C0">Choose these names to make documentation easy to read. For example, these names make documentation read naturally:</span></i>

    ```swift
    ✅
    /// Return an `Array` containing the elements of `self`
    /// that satisfy `predicate`.
    func filter(_ predicate: (Element) -> Bool) -> [Generator.Element]

    /// Replace the given `subRange` of elements with `newElements`.
    mutating func replaceRange(_ subRange: Range, with newElements: [E])
    ```

    <i><span style="color: #C0C0C0">These, however, make the documentation awkward and ungrammatical:</span></i>

    ```swift
    ❌
    /// Return an `Array` containing the elements of `self`
    /// that satisfy `includedInResult`.
    func filter(_ includedInResult: (Element) -> Bool) -> [Generator.Element]

    /// Replace the range of elements indicated by `r` with
    /// the contents of `with`.
    mutating func replaceRange(_ r: Range, with: [E])
    ```


- <i><span style="color: #C0C0C0">**Take advantage of defaulted parameters** when it simplifies common uses. Any parameter with a single commonly-used value is a candidate for a default.</span></i>

    <i><span style="color: #C0C0C0">Default arguments improve readability by hiding irrelevant information. For example:</span></i>

    ```swift
    ❌
    let order = lastName.compare(
    royalFamilyName, options: [], range: nil, locale: nil)
    ```

    <i><span style="color: #C0C0C0">can become the much simpler:</span><i>

    ```swift
    ✅
    let order = lastName.compare(royalFamilyName)
    ```

    <i><span style="color: #C0C0C0">Default arguments are generally preferable to the use of method families, because they impose a lower cognitive burden on anyone trying to understand the API.</span></i>

    ```swift
    ✅
    extension String {
        /// ...description...
        public func compare(
            _ other: String, options: CompareOptions = [],
            range: Range? = nil, locale: Locale? = nil
        ) -> Ordering
    }
    ```

    <i><span style="color: #C0C0C0">The above may not be simple, but it is much simpler than:</span></i>

    ```swift
    ❌
    extension String {
        /// ...description 1...
        public func compare(_ other: String) -> Ordering
        /// ...description 2...
        public func compare(_ other: String, options: CompareOptions) -> Ordering
        /// ...description 3...
        public func compare(
            _ other: String, options: CompareOptions, range: Range) -> Ordering
        /// ...description 4...
        public func compare(
            _ other: String, options: StringCompareOptions,
            range: Range, locale: Locale) -> Ordering
    }
    ```

    <i><span style="color: #C0C0C0">Every member of a method family needs to be separately documented and understood by users. To decide among them, a user needs to understand all of them, and occasional surprising relationships—for example, foo(bar: nil) and foo() aren’t always synonyms—make this a tedious process of ferreting out minor differences in mostly identical documentation. Using a single method with defaults provides a vastly superior programmer experience.</span></i>


- <i><span style="color: #C0C0C0">**Prefer to locate parameters with defaults toward the end** of the parameter list. Parameters without defaults are usually more essential to the semantics of a method, and provide a stable initial pattern of use where methods are invoked.</span></i>

<br>

## Argument Labels(전달인자 레이블)

```swift
func move(from start: Point, to end: Point)
x.move(from: x, to: y) 
```

- <i><span style="color: #C0C0C0">**Omit all labels when arguments can’t be usefully distinguished,** e.g. min(number1, number2), zip(sequence1, sequence2).</span></i>

- **전달인자 레이블이 전달인자들을 유용하게 구분하지 못하는 경우 모든 전달인자 레이블을 무시합니다.** 예시 : *min(number1, number2), zip(sequence1, sequence2)*.

- <i><span style="color: #C0C0C0">**In initializers that perform value preserving type conversions, omit the first argument label,** e.g. Int64(someUInt32)</span></i>

- <i>**값은 보존하고 타입만 변경하는 이니셜라이저의 경우 첫 번째 전달인자 레이블을 무시합니다.** 예시 : *Int64(someUInt32)* </i>

    <i><span style="color: #C0C0C0">The first argument should always be the source of the conversion.</span></i>
    
    <i>타입 변환의 대상은 반드시 첫 번째 전달인자여야 합니다.</i>
    ```swift
    extension String {
    // Convert `x` into its textual representation in the given radix
    // `x`를 주어진 기수(기초가되는 수, radix)내에서 문자적인 표현으로 바꿉니다. 
    init(_ x: BigInt, radix: Int = 10)   ← Note the initial underscore (초기의 언더스코어를 참고합니다.)
    }

    text = "The value is: "
    text += String(veryLargeNumber)
    text += " and in hexadecimal, it's"
    text += String(veryLargeNumber, radix: 16)
    ```

    <i><span style="color: #C0C0C0">In “narrowing” type conversions, though, a label that describes the narrowing is recommended.</span></i>
    
    <i>그러나 타입변환 대상의 범위가 한정된 경우, 전달인자 레이블을 통해 그 범위를 나타내는 것을 추천합니다. </i>

    ```swift
    extension UInt32 {
    /// Creates an instance having the specified `value`.
    /// 특정한 값을 가지는 인스턴스를 만듭니다. 
    init(_ value: Int16)            ← Widening, so no label(범위가 넓기 때문에 레이블이 없습니다.)
    /// Creates an instance having the lowest 32 bits of `source`.
    /// 가장 낮은 32비트를 자원으로 가지는 인스턴스를 만듭니다.
    init(truncating source: UInt64)
    /// Creates an instance having the nearest representable
    /// approximation of `valueToApproximate`.
    /// `valueToApproximate`의 표현가능하고 
    /// 가장 근접한 추정치를 가지는 인스턴스를 만듭니다. 
    init(saturating valueToApproximate: UInt64)
    }
    ```

    > <i><span style="color: #C0C0C0">A value preserving type conversion is a [monomorphism](https://en.wikipedia.org/wiki/Monomorphism), i.e. every difference in the value of the source results in a difference in the value of the result. For example, conversion from Int8 to Int64 is value preserving because every distinct Int8 value is converted to a distinct Int64 value. Conversion in the other direction, however, cannot be value preserving: Int64 has more possible values than can be represented in an Int8.</span></i>
    > 
    > <i>값을 보존하면서 타입을 바꾸는것을 [단형성](https://en.wikipedia.org/wiki/Monomorphism)이라고 합니다. 즉 타입변환 대상의 값이 다르면 결과도 다릅니다. 예를 들어 Int8에서 Int64로 타입을 바꾸는 경우 값이 보존되지만 반대의 경우엔 값이 보존되지 않습니다. 왜냐하면 Int64는 Int8타입으로 바뀌면서 다양한 값으로 표현될 수 있기 때문입니다.</i>
    > 
    > <i><span style="color: #C0C0C0">Note: the ability to retrieve the original value has no bearing on whether a conversion is value preserving.</span></i>
    > 
    > <i>참고 : 원래의 값을 검색할 수 있는지의 여부는 (타입)변환이 값을 보존하는지의 여부와 관계없습니다.</i>


- <i><span style="color: #C0C0C0">**When the first argument forms part of a [prepositional phrase](https://en.wikipedia.org/wiki/Adpositional_phrase#Prepositional_phrases), give it an argument label.** The argument label should normally begin at the [preposition](https://en.wikipedia.org/wiki/Preposition_and_postposition), e.g. x.removeBoxes(havingLength: 12).</span></i>

- <i>**첫 번째 전달인자가 [전치사구](https://en.wikipedia.org/wiki/Adpositional_phrase#Prepositional_phrases)의 일부라면, 전달인자 레이블을 줍니다.** 이 때 이 전달인자 레이블은 반드시 [전치사](https://en.wikipedia.org/wiki/Preposition_and_postposition) 위치에서 시작합니다. 예 : x.removeBoxes(havingLength: 12)</i>

    <i><span style="color: #C0C0C0">An exception arises when the first two arguments represent parts of a single abstraction.</span></i>
    
    <i>예외가 발생하는 경우가 있는데 첫 번째 두 전달인자들이 단일 추상화의 일부로 표현될 때 입니다.</i>
    ```swift
    ❌
    a.move(toX: b, y: c)
    a.fade(fromRed: b, green: c, blue: d)
    ```

    <i><span style="color: #C0C0C0">In such cases, begin the argument label after the preposition, to keep the abstraction clear.</span></i>
    
    <i>이런 경우에는 추상화를 정확히 하기 위해 전달인자 레이블을 전치사 다음에 시작하도록 합니다.</i>

    ```swift
    ✅
    a.moveTo(x: b, y: c)
    a.fadeFrom(red: b, green: c, blue: d)
    ```

- <i><span style="color: #C0C0C0">**Otherwise, if the first argument forms part of a grammatical phrase, omit its label,** appending any preceding words to the base name, e.g. x.addSubview(y)</span></i>

- <i> **첫 번째 전달인자가 문법구문의 일부라면 전달인자 레이블을 무시하고** 기본이름에 선행단어를 추가합니다. 예 :x.addSubview(y) </i>
    
    <i><span style="color: #C0C0C0">This guideline implies that if the first argument doesn’t form part of a grammatical phrase, it should have a label.</span></i>
    
    <i>이 가이드라인이 의미하는 것은 첫 번째 전달인자가 문법구문의 일부가 아니라면 반드시 레이블을 가져야한다는것 입니다. </i>
    ```swift
    ✅
    view.dismiss(animated: false)
    let text = words.split(maxSplits: 12)
    let studentsByName = students.sorted(isOrderedBefore: Student.namePrecedes)
    ```

    <i><span style="color: #C0C0C0">Note that it’s important that the phrase convey the correct meaning. The following would be grammatical but would express the wrong thing.</span></i>
    
    <i>문구가 올바른 의미를 전달하고 있는지가 중요합니다. 아래의 내용은 문법적으로는 맞지만 잘못된 의미입니다.</i>
    ```swift
    ❌
    view.dismiss(false)   Don't dismiss? Dismiss a Bool? (해제하지 말아라? Bool을 해제해라?)
    words.split(12)       Split the number 12?(숫자 12를 나눠라?)
    ```

    <i><span style="color: #C0C0C0">Note also that arguments with default values can be omitted, and in that case do not form part of a grammatical phrase, so they should always have labels.</span></i>
    
    <i>또한 기본값을 가지는 전달인자는 무시될 수 있습니다. 이 경우에는 문법구문의 일부가 아니기 때문에 항상 레이블을 가져야 합니다.</i> 
- <i><span style="color: #C0C0C0">**Label all other arguments.**</span></i>

- <i>**레이블은 모두 다른 전달인자들입니다.**</i>
