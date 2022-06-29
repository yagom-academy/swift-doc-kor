
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

## Parameters (매개변수)

```swift
func move(from start: Point, to end: Point)
```

- <i><span style="color: #C0C0C0">**Choose parameter names to serve documentation.** Even though parameter names do not appear at a function or method’s point of use, they play an important explanatory role.</span></i>
- <span>**문서를 제공할 매개변수 이름을 선택합니다.** 매개변수 이름이 함수 또는 메서드의 사용 시점에 나타나지 않더라도 설명에 중요한 역할을 합니다.</span>

    <i><span style="color: #C0C0C0">Choose these names to make documentation easy to read. For example, these names make documentation read naturally:</span></i>
    <span>문서를 쉽게 읽을 수 있도록 매개변수 이름을 선택합니다. 예를 들어, 매개변수 이름은 자연스럽게 문서를 읽을 수 있도록 만듭니다:</span>
    
    ```swift
    ✅
    /// Return an `Array` containing the elements of `self`
    /// that satisfy `predicate`.
    /// 'predicate'를 만족시키는 'self'의 요소를 포함하는 'Array'를 반환
    func filter(_ predicate: (Element) -> Bool) -> [Generator.Element]
    
    /// Replace the given `subRange` of elements with `newElements`.
    /// 주어진 요소의 'subRange'를 'newElements'로 변환
    mutating func replaceRange(_ subRange: Range, with newElements: [E])
    ```
    
    <i><span style="color: #C0C0C0">These, however, make the documentation awkward and ungrammatical:</span></i>
    <span>그러나 매개변수는 문서를 어색하고 문법에 맞지 않게 만듭니다:</span>
    
    ```swift
    ❌
    /// Return an `Array` containing the elements of `self`
    /// that satisfy `includedInResult`.
    /// 'includedInResult'를 만족시키는 'self'의 요소를 포함하는 'Array'를 반환
    func filter(_ includedInResult: (Element) -> Bool) -> [Generator.Element]
    
    /// Replace the range of elements indicated by `r` with
    /// the contents of `with`.
    /// 'r'로 표시된 요소의 범위를 'with'의 내용으로 변환
    mutating func replaceRange(_ r: Range, with: [E])
    ```


- <i><span style="color: #C0C0C0">**Take advantage of defaulted parameters** when it simplifies common uses. Any parameter with a single commonly-used value is a candidate for a default.</span></i>
- <span>**기본 매개변수는** 일반적인 사용을 단순화할 때 **이점을 가집니다.** 일반적으로 사용되는 단일 값을 가진 모든 매개변수는 기본값 후보입니다.</span>
  
  <i><span style="color: #C0C0C0">Default arguments improve readability by hiding irrelevant information. For example:</span></i>
  <span>기본 인자는 관련 없는 정보를 숨김으로 가독성을 향상합니다. 예를 들어:</span>
  
  
    ```swift
    ❌
    let order = lastName.compare(
    royalFamilyName, options: [], range: nil, locale: nil)
    ```
  
  <i><span style="color: #C0C0C0">can become the much simpler:</span></i>
  <span>훨씬 더 단순해질 수 있습니다:</span>
  
    ```swift
    ✅
    let order = lastName.compare(royalFamilyName)
    ```
  
  <i><span style="color: #C0C0C0">Default arguments are generally preferable to the use of method families, because they impose a lower cognitive burden on anyone trying to understand the API.</span></i>
  <span>기본 인자는 일반적으로 메서드 집합을 사용하는 것보다 선호되는데, API를 이해하려는 모든 사람에게 인식의 부담을 낮춰줍니다.</span>
  
    ```swift
    ✅
    extension String {
        /// ...description...
        /// ...설명...
        public func compare(
            _ other: String, options: CompareOptions = [],
            range: Range? = nil, locale: Locale? = nil
        ) -> Ordering
    }
    ```
  
  <i><span style="color: #C0C0C0">The above may not be simple, but it is much simpler than:</span></i>
  <span>위의 내용은 간단하지 않을 수 있지만, 다음보다는 더 간단합니다:</span>
  
    ```swift
    ❌
    extension String {
        /// ...description 1...
        /// ...설명 1...
        public func compare(_ other: String) -> Ordering
        /// ...description 2...
        /// ...설명 2...
        public func compare(_ other: String, options: CompareOptions) -> Ordering
        /// ...description 3...
        /// ...설명 3...
        public func compare(
            _ other: String, options: CompareOptions, range: Range) -> Ordering
        /// ...description 4...
        /// ...설명 4...
        public func compare(
            _ other: String, options: StringCompareOptions,
            range: Range, locale: Locale) -> Ordering
    }
    ```
  
  <i><span style="color: #C0C0C0">Every member of a method family needs to be separately documented and understood by users. To decide among them, a user needs to understand all of them, and occasional surprising relationships—for example, foo(bar: nil) and foo() aren’t always synonyms—make this a tedious process of ferreting out minor differences in mostly identical documentation. Using a single method with defaults provides a vastly superior programmer experience.</span></i>
  <span>메서드 집합의 모든 구성은 별도의 문서화가 필요하며 사용자가 이해해야 합니다. 그중 하나를 결정하기 위해서는 사용자가 메서드 집합 전부를 이해할 필요가 있습니다. 그리고 때때로 놀라운 관계-예시로, foo(bar: nil)과 foo()는 항상 동의어가 아닙니다-는 거의 동일한 문서에서 작은 차이를 찾아내는 지루한 과정을 만듭니다. 기본값과 함께 단일 메서드를 사용하면 훨씬 더 우수한 프로그래머 경험을 제공합니다.</span>
  
- <i><span style="color: #C0C0C0">**Prefer to locate parameters with defaults toward the end** of the parameter list. Parameters without defaults are usually more essential to the semantics of a method, and provide a stable initial pattern of use where methods are invoked.</span></i>
- <span>매개변수 목록의 **마지막 방향으로 기본값이 있는 매개변수를 위치시키는 것을 선호합니다.** 기본값이 없는 매개변수는 일반적으로 메서드의 의미가 더 필수적이고, 메서드가 호출되는 곳에서 안정적인 초기 패턴을 제공합니다.</span>

<br>

## Argument Labels(전달인자 레이블)

```swift
func move(from start: Point, to end: Point)
x.move(from: x, to: y) 
```

- <i><span style="color: #C0C0C0">**Omit all labels when arguments can’t be usefully distinguished,** e.g. min(number1, number2), zip(sequence1, sequence2).</span></i>

- **전달인자 레이블이 전달인자들을 유용하게 구분하지 못하는 경우 모든 전달인자 레이블을 무시합니다.** 예시 : min(number1, number2), zip(sequence1, sequence2).

- <i><span style="color: #C0C0C0">**In initializers that perform value preserving type conversions, omit the first argument label,** e.g. Int64(someUInt32)</span></i>

- 값은 보존하고 타입만 변경하는 이니셜라이저의 경우 첫 번째 전달인자 레이블을 무시합니다. 예시 : Int64(someUInt32)

    <i><span style="color: #C0C0C0">The first argument should always be the source of the conversion.</span></i>
    
    타입 변환의 대상은 반드시 첫 번째 전달인자여야 합니다.
    
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
    
    그러나 타입변환 대상의 범위가 한정된 경우, 전달인자 레이블을 통해 그 범위를 나타내는 것을 추천합니다.

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
    > 값을 보존하면서 타입을 바꾸는것을 [단형성](https://en.wikipedia.org/wiki/Monomorphism)이라고 합니다. 즉 타입변환 대상의 값이 다르면 결과도 다릅니다. 예를 들어 Int8에서 Int64로 타입을 바꾸는 경우 값이 보존되지만 반대의 경우엔 값이 보존되지 않습니다. 왜냐하면 Int64는 Int8타입으로 바뀌면서 다양한 값으로 표현될 수 있기 때문입니다.
    > 
    > <i><span style="color: #C0C0C0">Note: the ability to retrieve the original value has no bearing on whether a conversion is value preserving.</span></i>
    > 
    > 참고 : 원래의 값을 검색할 수 있는지의 여부는 (타입)변환이 값을 보존하는지의 여부와 관계없습니다.


- <i><span style="color: #C0C0C0">**When the first argument forms part of a [prepositional phrase](https://en.wikipedia.org/wiki/Adpositional_phrase#Prepositional_phrases), give it an argument label.** The argument label should normally begin at the [preposition](https://en.wikipedia.org/wiki/Preposition_and_postposition), e.g. x.removeBoxes(havingLength: 12).</span></i>

- **첫 번째 전달인자가 [전치사구](https://en.wikipedia.org/wiki/Adpositional_phrase#Prepositional_phrases)의 일부라면, 전달인자 레이블을 줍니다.** 이 때 이 전달인자 레이블은 반드시 [전치사](https://en.wikipedia.org/wiki/Preposition_and_postposition) 위치에서 시작합니다. 예 : x.removeBoxes(havingLength: 12)

    <i><span style="color: #C0C0C0">An exception arises when the first two arguments represent parts of a single abstraction.</span></i>
    
    예외가 발생하는 경우가 있는데 첫 번째 두 전달인자들이 단일 추상화의 일부로 표현될 때 입니다.
    ```swift
    ❌
    a.move(toX: b, y: c)
    a.fade(fromRed: b, green: c, blue: d)
    ```

    <i><span style="color: #C0C0C0">In such cases, begin the argument label after the preposition, to keep the abstraction clear.</span></i>
    
    이런 경우에는 추상화를 정확히 하기 위해 전달인자 레이블을 전치사 다음에 시작하도록 합니다.

    ```swift
    ✅
    a.moveTo(x: b, y: c)
    a.fadeFrom(red: b, green: c, blue: d)
    ```

- <i><span style="color: #C0C0C0">**Otherwise, if the first argument forms part of a grammatical phrase, omit its label,** appending any preceding words to the base name, e.g. x.addSubview(y)</span></i>

- **첫 번째 전달인자가 문법구문의 일부라면 전달인자 레이블을 무시하고** 기본이름에 선행단어를 추가합니다. 예 :x.addSubview(y)
    
    <i><span style="color: #C0C0C0">This guideline implies that if the first argument doesn’t form part of a grammatical phrase, it should have a label.</span></i>
    
    이 가이드라인이 의미하는 것은 첫 번째 전달인자가 문법구문의 일부가 아니라면 반드시 레이블을 가져야한다는것 입니다. 
    
    ```swift
    ✅
    view.dismiss(animated: false)
    let text = words.split(maxSplits: 12)
    let studentsByName = students.sorted(isOrderedBefore: Student.namePrecedes)
    ```

    <i><span style="color: #C0C0C0">Note that it’s important that the phrase convey the correct meaning. The following would be grammatical but would express the wrong thing.</span></i>
    
    문구가 올바른 의미를 전달하고 있는지가 중요합니다. 아래의 내용은 문법적으로는 맞지만 잘못된 의미입니다.
    
    ```swift
    ❌
    view.dismiss(false)   Don't dismiss? Dismiss a Bool? (해제하지 말아라? Bool을 해제해라?)
    words.split(12)       Split the number 12?(숫자 12를 나눠라?)
    ```

    <i><span style="color: #C0C0C0">Note also that arguments with default values can be omitted, and in that case do not form part of a grammatical phrase, so they should always have labels.</span></i>
    
    또한 기본값을 가지는 전달인자는 무시될 수 있습니다. 이 경우에는 문법구문의 일부가 아니기 때문에 항상 레이블을 가져야 합니다.
    
- <i><span style="color: #C0C0C0">**Label all other arguments.**</span></i>

- **(그외) 다른 전달인자들은 이름붙이세요.**
