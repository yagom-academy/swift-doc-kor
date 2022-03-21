
# Conventions

## General Conventions (일반 규칙)

- <i><span style="color: #C0C0C0">**Document the complexity of any computed property that is not O(1).** People often assume that property access involves no significant computation, because they have stored properties as a mental model. Be sure to alert them when that assumption may be violated.</span></i>

- **복잡도가 O(1)이 아닌 연산 프로퍼티는 복잡도를 기록합니다.** 사람들은 보통 저장 프로퍼티를 떠올리기 때문에 프로퍼티에 접근하는 것에 엄청난 연산이 필요하다고 생각하지 않습니다. 프로퍼티에 접근할 때 추가적인 비용이 발생하는 경우에는 반드시 알려야 합니다.

- <i><span style="color: #C0C0C0">**Prefer methods and properties to free functions.** Free functions are used only in special cases:</span></i>

- **자유 함수보다 메서드와 프로퍼티를 선호합니다.** 자유 함수는 특수한 경우에만 사용됩니다:

    1. <i><span style="color: #C0C0C0">When there’s no obvious self:</span></i>
    명확한 self가 없을 때:
        ```swift
        min(x, y, z)
        ```

    2. <i><span style="color: #C0C0C0">When the function is an unconstrained generic:</span></i>
    제약 없는 제네릭 함수일 때:
        ```swift
        print(x)
        ```

    3. <i><span style="color: #C0C0C0">When function syntax is part of the established domain notation:</span></i>
    함수 구문이 특정한 도메인 표기법의 일부일 때:
        ```swift
        sin(x)
        ```

- <i><span style="color: #C0C0C0">**Follow case conventions.** Names of types and protocols are UpperCamelCase. Everything else is lowerCamelCase.</span></i>

- **대소문자 표기법을 따릅니다.** 타입과 프로토콜의 이름은 UpperCamelCase, 그 외 모든 것은 lowerCamelCase로 표기합니다.  

    <i><span style="color: #C0C0C0">[Acronyms and initialisms](https://en.wikipedia.org/wiki/Acronym) that commonly appear as all upper case in American English should be uniformly up- or down-cased according to case conventions:</span></i>

    단어로 발음하는 두문자어와 한 글자씩 발음하는 두문자어는 미국 영어에서 일반적으로 대문자로 씁니다. 두문자어는 규칙에 따라 일관되게 대문자 또는 소문자로 나타내야 합니다:

    ```swift
    var utf8Bytes: [UTF8.CodeUnit]
    var isRepresentableAsASCII = true
    var userSMTPServer: SecureSMTPServer
    ```

    <i><span style="color: #C0C0C0">Other acronyms should be treated as ordinary words:</span></i>

    그 외의 단어로 발음하는 두문자어는 일반적인 단어처럼 나타내야 합니다:

    ```swift
    var radarDetector: RadarScanner
    var enjoysScubaDiving = true
    ```

- <i><span style="color: #C0C0C0">**Methods can share a base name** when they share the same basic meaning or when they operate in distinct domains.</span></i>

- 기본적인 의미가 같거나 서로 다른 도메인에서 동작할 때 **메서드들은 기본이 되는 이름을 공유할 수 있습니다.** 

    <i><span style="color: #C0C0C0">For example, the following is encouraged, since the methods do essentially the same things:</span></i>

    예를 들어, 다음 메서드들은 본질적으로 같은 일을 하고 있으므로 같은 이름을 사용하는 것이 권장됩니다:

    ```swift
    ✅
    extension Shape {
        /// Returns `true` iff `other` is within the area of `self`.
        /// `other`가 `self`의 영역 내에 있는 경우 `true`를 반환합니다.
        func contains(_ other: Point) -> Bool { ... }

        /// Returns `true` iff `other` is entirely within the area of `self`.
        /// `other`가 완전히 `self`의 영역 내에 있는 경우 `true`를 반환합니다.
        func contains(_ other: Shape) -> Bool { ... }

        /// Returns `true` iff `other` is within the area of `self`.
        /// `other`가 `self`의 영역 내에 있는 경우 `true`를 반환합니다.
        func contains(_ other: LineSegment) -> Bool { ... }
    }
    ```

    <i><span style="color: #C0C0C0">And since geometric types and collections are separate domains, this is also fine in the same program:</span></i>

    위의 기하학 타입과 컬렉션은 다른 도메인이라서, 같은 프로그램 내에서 이름이 같아도 괜찮습니다:

    ```swift
    ✅
    extension Collection where Element : Equatable {
        /// Returns `true` iff `self` contains an element equal to
        /// `sought`.
        /// `self`에 `sought`와 동일한 요소가 포함된 경우 `true`를 반환합니다.
        func contains(_ sought: Element) -> Bool { ... }
    }
    ```

    <i><span style="color: #C0C0C0">However, these index methods have different semantics, and should have been named differently:</span></i>
    
    하지만 이 index 메서드들은 다른 의미를 가지고 있으므로 다른 이름을 지어야 합니다:

    ```swift
    ❌
    extension Database {
        /// Rebuilds the database's search index
        /// 데이터베이스의 검색 인덱스 리빌드
        func index() { ... }

        /// Returns the `n`th row in the given table.
        /// 주어진 테이블의 `n`번째 행을 반환합니다.
        func index(_ n: Int, inTable: TableID) -> TableRow { ... }
    }
    ```

    <i><span style="color: #C0C0C0">Lastly, avoid “overloading on return type” because it causes ambiguities in the presence of type inference.</span></i>

    마지막으로 “반환 타입만 재정의”하는 방식은 타입 추론 시 모호해질 수 있으므로 피합니다.

    ```swift
    ❌
    extension Box {
        /// Returns the `Int` stored in `self`, if any, and
        /// `nil` otherwise.
        /// `self`에 저장된 `Int`가, 만약에 있으면, 반환하고
        /// 그렇지 않으면 `nil`을 반환합니다.
        func value() -> Int? { ... }

        /// Returns the `String` stored in `self`, if any, and
        /// `nil` otherwise.
        /// `self`에 저장된 `String`이, 만약에 있으면, 반환하고
        /// 그렇지 않으면 `nil`을 반환합니다.
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

## Argument Labels

```swift
func move(from start: Point, to end: Point)
x.move(from: x, to: y) 
```

- <i><span style="color: #C0C0C0">**Omit all labels when arguments can’t be usefully distinguished,** e.g. min(number1, number2), zip(sequence1, sequence2).</span></i>

- <i><span style="color: #C0C0C0">**In initializers that perform value preserving type conversions, omit the first argument label,** e.g. Int64(someUInt32)</span></i>

    <i><span style="color: #C0C0C0">The first argument should always be the source of the conversion.</span></i>

    ```swift
    extension String {
    // Convert `x` into its textual representation in the given radix
    init(_ x: BigInt, radix: Int = 10)   ← Note the initial underscore
    }

    text = "The value is: "
    text += String(veryLargeNumber)
    text += " and in hexadecimal, it's"
    text += String(veryLargeNumber, radix: 16)
    ```

    <i><span style="color: #C0C0C0">In “narrowing” type conversions, though, a label that describes the narrowing is recommended.</span></i>

    ```swift
    extension UInt32 {
    /// Creates an instance having the specified `value`.
    init(_ value: Int16)            ← Widening, so no label
    /// Creates an instance having the lowest 32 bits of `source`.
    init(truncating source: UInt64)
    /// Creates an instance having the nearest representable
    /// approximation of `valueToApproximate`.
    init(saturating valueToApproximate: UInt64)
    }
    ```

    > <i><span style="color: #C0C0C0">A value preserving type conversion is a [monomorphism](https://en.wikipedia.org/wiki/Monomorphism), i.e. every difference in the value of the source results in a difference in the value of the result. For example, conversion from Int8 to Int64 is value preserving because every distinct Int8 value is converted to a distinct Int64 value. Conversion in the other direction, however, cannot be value preserving: Int64 has more possible values than can be represented in an Int8.</span></i>
    > 
    > <i><span style="color: #C0C0C0">Note: the ability to retrieve the original value has no bearing on whether a conversion is value preserving.</span></i>


- <i><span style="color: #C0C0C0">**When the first argument forms part of a [prepositional phrase](https://en.wikipedia.org/wiki/Adpositional_phrase#Prepositional_phrases), give it an argument label.** The argument label should normally begin at the [preposition](https://en.wikipedia.org/wiki/Preposition_and_postposition), e.g. x.removeBoxes(havingLength: 12).</span></i>

    <i><span style="color: #C0C0C0">An exception arises when the first two arguments represent parts of a single abstraction.</span></i>

    ```swift
    ❌
    a.move(toX: b, y: c)
    a.fade(fromRed: b, green: c, blue: d)
    ```

    <i><span style="color: #C0C0C0">In such cases, begin the argument label after the preposition, to keep the abstraction clear.</span></i>

    ```swift
    ✅
    a.moveTo(x: b, y: c)
    a.fadeFrom(red: b, green: c, blue: d)
    ```

- <i><span style="color: #C0C0C0">**Otherwise, if the first argument forms part of a grammatical phrase, omit its label,** appending any preceding words to the base name, e.g. x.addSubview(y)</span></i>

    <i><span style="color: #C0C0C0">This guideline implies that if the first argument doesn’t form part of a grammatical phrase, it should have a label.</span></i>

    ```swift
    ✅
    view.dismiss(animated: false)
    let text = words.split(maxSplits: 12)
    let studentsByName = students.sorted(isOrderedBefore: Student.namePrecedes)
    ```

    <i><span style="color: #C0C0C0">Note that it’s important that the phrase convey the correct meaning. The following would be grammatical but would express the wrong thing.</span></i>

    ```swift
    ❌
    view.dismiss(false)   Don't dismiss? Dismiss a Bool?
    words.split(12)       Split the number 12?
    ```

    <i><span style="color: #C0C0C0">Note also that arguments with default values can be omitted, and in that case do not form part of a grammatical phrase, so they should always have labels.</span></i>

- <i><span style="color: #C0C0C0">**Label all other arguments.**</span></i>

