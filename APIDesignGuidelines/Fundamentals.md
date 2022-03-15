
# Fundamentals (핵심 개념)

- <i><span style="color: #C0C0C0">**Clarity at the point of use** is your most important goal. Entities such as methods and properties are declared only once but used repeatedly. Design APIs to make those uses clear and concise. When evaluating a design, reading a declaration is seldom sufficient; always examine a use case to make sure it looks clear in context.</span></i>

- API 설계의 가장 중요한 목적은 **사용하는 시점에서의 명료성**입니다. 메서드나 프로퍼티와 같은 개체는 한 번 선언해두면 반복적으로 사용이 가능합니다. API는 이러한 개체들을 명확하고 간결하게 사용할 수 있도록 설계해야합니다. API 설계를 제대로 하였는지 평가할 때는 코드의 선언부를 읽는 것만으로는 충분하지 않습니다. 항상 문맥에 알맞도록 명확하게 설계하였는지 실질적인 사용 예시를 통해 확인해보아야합니다.


- <i><span style="color: #C0C0C0">**Clarity is more important than brevity.** Although Swift code can be compact, it is a non-goal to enable the smallest possible code with the fewest characters. Brevity in Swift code, where it occurs, is a side-effect of the strong type system and features that naturally reduce boilerplate.</span></i>

- **명료성은 간결성보다 더 중요합니다.** Swift 코드를 간결하게 작성할 수 있지만, 단순히 몇개의 문자들만 사용하여 가장 적은 코드를 작성하는 것이 목표가 아닙니다. Swift에서 간결성은 강력한 타입 시스템의 부수효과이고 반복되는 인용구를 자연스럽게 줄일 수 있는 기능입니다. 

- <i><span style="color: #C0C0C0">**Write a documentation comment for every declaration.** Insights gained by writing documentation can have a profound impact on your design, so don’t put it off.</span></i>   

- **모든 선언문에 문서 주석을 작성합니다.** 주석을 작성하면서 얻은 통찰력은 당신의 API 설계에 엄청난 영향을 줄 수 있습니다. 그러니 미루지 말고 주석을 작성하세요.

  > <i><span style="color: #C0C0C0">If you are having trouble describing your API’s functionality in simple terms, you may have designed the wrong API.</span></i>
  
  > 만약 당신이 API 기능을 간단한 용어로 설명하는 게 어렵다면, 잘못된 API를 설계했을 수도 있습니다.

  - <i><span style="color: #C0C0C0">**Use Swift’s [dialect of Markdown](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/).**</span></i>

  - **Swift에서 지원하는 [마크다운 형식](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/)을 이용하세요.**

  - <i><span style="color: #C0C0C0">**Begin with a summary** that describes the entity being declared. Often, an API can be completely understood from its declaration and its summary.</span></i>
  - 선언된 개체를 설명하는 요약부터 시작하세요. 간혹, API는 선언부와 요약을 읽는 것만으로도 이해가 되기도 합니다.
    ```swift
    /// Returns a "view" of `self` containing the same elements in
    /// reverse order.
    /// 동일한 요소들을 포함하는 `self`의 "view"를 역순으로 반환.
    func reversed() -> ReverseCollection
    ```

    - <i><span style="color: #C0C0C0">**Focus on the summary**; it’s the most important part. Many excellent documentation comments consist of nothing more than a great summary.</span></i>
    - **요약에 초점을 맞추세요.** 요약은 정말 중요한 부분입니다. 좋은 문서 주석들은 훌륭한 요약으로 이루어져 있습니다.

    - <i><span style="color: #C0C0C0">**Use a single sentence fragment** if possible, ending with a period. Do not use a complete sentence.</span></i>
    - **가능하다면, 한 문장 구성 단위(구, 절)를 사용하고 끝에 마침표로 끝내세요.** 완전한 문장으로 사용하는 것은 지양합니다.

    - <i><span style="color: #C0C0C0">**Describe what a function or method does and what it *returns*,** omitting null effects and Void returns:</span></i>
    - **함수 또는 메서드가 하는 일이 무엇이고, 어떤 것을 반환하는지 설명하고** null 효과와 Void 반환에 대한 설명은 생략하세요.

    ```swift
    /// Inserts `newHead` at the beginning of `self`.
    /// `self`의 시작부분에 `newHead` 삽입
    mutating func prepend(_ newHead: Int)

    /// Returns a `List` containing `head` followed by the elements
    /// of `self`.
    /// `self`의 요소에 있는 'head`가 포함된 `List` 반환
    func prepending(_ head: Element) -> List

    /// Removes and returns the first element of `self` if non-empty;
    /// returns `nil` otherwise.
    /// 비어있지 않다면 `self`의 첫 번째 요소를 반환 및 제거하고, 비어 있다면 `nil` 반환
    mutating func popFirst() -> Element?
    ```

    <i><span style="color: #C0C0C0">Note: in rare cases like popFirst above, the summary is formed of multiple sentence fragments separated by semicolons.</span></i>
    
    주의: 드문 경우지만 popFirst처럼, 세미콜론을 이용해 여러 문장 구성 단위(구, 절) 형태로 요약을 작성할 수 있습니다.
    
    - <i><span style="color: #C0C0C0">**Describe what a subscript accesses**:</span></i>
    - **subscript가 어디에 접근하는지 설명합니다.**
    ```swift
    /// Accesses the `index`th element.
    /// `index` 번째 요소에 접근한다.
    subscript(index: Int) -> Element { get set }
    ```

    - <i><span style="color: #C0C0C0">**Describe what an initializer creates**:</span></i>
    - 이니셜라이저가 무엇을 생성하는 지 설명합니다.
    ```swift
    /// Creates an instance containing `n` repetitions of `x`.
    /// `x`를 `n`번 반복하는 인스턴스를 생성합니다.
    init(count n: Int, repeatedElement x: Element)
    ```

    - <i><span style="color: #C0C0C0">For all other declarations, **describe what the declared entity *is*.**</span></i>
    - 다른 모든 선언의 경우, 선언된 개체가 무엇인지 설명합니다.
    ```swift
    /// A collection that supports equally efficient insertion/removal
    /// at any position.
    /// 어느 위치에서도 동일하고 효율적으로 삽입/제거가 가능한 컬렉션
    struct List {

      /// The element at the beginning of `self`, or `nil` if self is
      /// empty.
      /// `self`의 첫 번째 요소가 비어있거나 self가 비어있다면 `nil`
      var first: Element?
      ...
    ```


  - <i><span style="color: #C0C0C0">**Optionally, continue** with one or more paragraphs and bullet items. Paragraphs are separated by blank lines and use complete sentences.</span></i>
  - **경우에 따라, 한 개 이상의 절과 글머리 기호로 이어갈 수 있습니다.** 절은 빈줄로 구별하고 완벽한 문장을 사용하세요.
    ```swift
    /// Writes the textual representation of each        ← Summary (요약)
    /// element of `items` to the standard output.
    /// 표준 출력에 `items`의 각 요소에 대한 텍스트 표현을 작성합니다.
    ///                                                  ← Blank line (빈줄)
    /// The textual representation for each item `x`     ← Additional discussion (추가 설명)
    /// is generated by the expression `String(x)`.
    /// 각 요소인 `x`에 대한 텍스트 표현은 `String(x)` 표현식에 의해 만들어집니다.
    ///
    /// - Parameter separator: text to be printed       ⎫
    ///   between items.                                ⎟
    /// - 매개변수 separator: 요소 사이에 출력되는 텍스트        ⎟
    /// - Parameter terminator: text to be printed      ⎬ Parameters section (매개변수 부분)
    ///   at the end.                                   ⎟
    /// - 매개변수 terminator: 끝부분에 출력되는 텍스트         ⎟  
    ///                                                 ⎭
    /// - Note: To print without a trailing             ⎫
    ///   newline, pass `terminator: ""`                ⎟
    /// - 주의: 줄 바꿈을 하지 않으려면 `terminater: ""` 입력   ⎟  
    ///                                                 ⎬ Symbol commands (기호 명령)
    /// - SeeAlso(참조): `CustomDebugStringConvertible`, ⎟
    ///   `CustomStringConvertible`, `debugPrint`.      ⎭
    public func print(
      _ items: Any..., separator: String = " ", terminator: String = "\n")
    ```

    - <i><span style="color: #C0C0C0">**Use recognized [symbol documentation markup elements](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW1)** to add information beyond the summary, whenever appropriate.</span></i>
    - 요약문 외에 추가적인 정보를 제공할 때는 적절하게 **[심볼 문서 마크업](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW1) 요소들을 사용하세요.**

    - <i><span style="color: #C0C0C0">**Know and use recognized bullet items with [symbol command syntax](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW13).** Popular development tools such as Xcode give special treatment to bullet items that start with the following keywords:</span></i>
    - **글머리 기호와 [심볼 명령 구문](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW13)을 알고 사용합니다.** Xcode 처럼 인기있는 개발도구는 아래의 키워드로 시작하는 글머리 기호를 특별하게 취급합니다.

|  [Attention](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Attention.html) |  [Author](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Author.html) |  [Authors](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Authors.html) |  [Bug](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Bug.html) |
| -- | -- | -- | -- |
|  [Complexity](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Complexity.html) |  [Copyright](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Copyright.html) |  [Date](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Date.html) |  [Experiment](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Experiment.html) | 
|  [Important](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Important.html) |  [Invariant](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Invariant.html) |  [Note](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Note.html) |  [Parameter](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Parameter.html) | 
|  [Parameters](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Parameters.html) |  [Postcondition](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Postcondition.html) |  [Precondition](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Precondition.html) |  [Remark](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Remark.html) | 
|  [Requires](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Requires.html) |  [Returns](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Returns.html) |  [SeeAlso](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/SeeAlso.html) |  [Since](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Since.html) | 
|  [Throws](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Throws.html) |  [Todo](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Todo.html) |  [Version](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Version.html) |  [Warning](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Warning.html) | 
