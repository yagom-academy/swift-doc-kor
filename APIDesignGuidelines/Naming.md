
# Naming

## Promote Clear Usage(명확하게 사용하기)

- <i><span style="color: #C0C0C0">**Include all the words needed to avoid ambiguity** for a person reading code where the name is used.</span></i>  
- 이름이 사용된 코드를 읽을 사람들을 위해 **모호하지 않도록 필요한 모든 단어를 포함합니다.** 

    <i><span style="color: #C0C0C0">For example, consider a method that removes the element at a given position within a collection.</span></i>  
    예를 들어, 컬렉션 내 주어진 위치에 있는 요소를 제거하는 메서드를 고려합니다.  
    
    ```swift
    ✅
    extension List {
    public mutating func remove(at position: Index) -> Element
    }
    employees.remove(at: x)
    ```

    <i><span style="color: #C0C0C0">If we were to omit the word at from the method signature, it could imply to the reader that the method searches for and removes an element equal to x, rather than using x to indicate the position of the element to remove.</span></i>  
    메서드 서명에서 at이라는 단어를 생략한다면, 읽는 사람에게 제거할 요소의 위치를 나타내기 위해 x를 사용했다기보다, x와 같은 요소를 검색하거나 제거한다는 의미일 수 있음을 시사합니다.  
    
    ```swift
    ❌
    employees.remove(x) // unclear: are we removing x? (불문명: x를 제거하는가?) 
    ```

- <i><span style="color: #C0C0C0">**Omit needless words.** Every word in a name should convey salient information at the use site.</span></i>  
- **불필요한 단어는 생략합니다.** 이름 내 모든 단어는 사용 장소에서 핵심적인 정보를 전달해야 합니다.   

    <i><span style="color: #C0C0C0">More words may be needed to clarify intent or disambiguate meaning, but those that are redundant with information the reader already possesses should be omitted. In particular, omit words that merely repeat type information.</span></i>  
    분명한 의도나 모호하지 않은 의미를 위해 더 많은 단어가 필요할 수 있지만, 읽는 사람이 이미 알고 있는 중복되는 정보는 생략합니다. 특히, 타입 정보만 반복하는 단어는 생략합니다.

    ```swift
    ❌
    public mutating func removeElement(_ member: Element) -> Element?

    allViews.removeElement(cancelButton)
    ```

    <i><span style="color: #C0C0C0">In this case, the word Element adds nothing salient at the call site. This API would be better:</span></i>  
    이 경우, Element 단어는 호출 장소에서 핵심적인 정보를 더하지 않습니다. 이 API가 더 나을 수 있습니다:  

    ```swift
    ✅
    public mutating func remove(_ member: Element) -> Element?

    allViews.remove(cancelButton) // clearer
    ```

    <i><span style="color: #C0C0C0">Occasionally, repeating type information is necessary to avoid ambiguity, but in general it is better to use a word that describes a parameter’s role rather than its type. See the next item for details.</span></i>  
    때때로 모호함을 피하기 위해 타입 정보를 반복하는 것이 필요하지만, 일반적으로 타입보다는 매개변수의 역할을 설명하는 단어를 사용하는 것이 더 낫습니다. 자세한 내용은 다음 항목을 참조합니다.    

- <i><span style="color: #C0C0C0">**Name variables, parameters, and associated types according to their roles,** rather than their type constraints.</span></i>  
- 타입 제약이 아닌 **해당 역할에 따라 변수, 매개변수, 및 연관 타입을 명명합니다.**   

    ```swift
    ❌
    var string = "Hello"
    protocol ViewController {
        associatedtype ViewType : View
    }
    class ProductionLine {
        func restock(from widgetFactory: WidgetFactory)
    }
    ```

    <i><span style="color: #C0C0C0">Repurposing a type name in this way fails to optimize clarity and expressivity. Instead, strive to choose a name that expresses the entity’s role.</span></i>  
    이런 방식으로 타입 이름을 고치는 것은 명확성과 표현성을 최적화하지 못합니다. 대신, 개체의 역할을 나타내는 이름을 선택하도록 노력합니다.  

    ```swift
    ✅
    var greeting = "Hello"
    protocol ViewController {
        associatedtype ContentView : View
    }
    class ProductionLine {
        func restock(from supplier: WidgetFactory)
    }
    ```

    <i><span style="color: #C0C0C0">If an associated type is so tightly bound to its protocol constraint that the protocol name is the role, avoid collision by appending Protocol to the protocol name:</span></i>  
    연관 타입이 프로토콜 이름이 역할일 정도로 프로토콜 제약에 매우 밀접하게 결합되었다면, 프로토콜 이름에 Protocol을 붙여 충돌을 피합니다.   

    ```swift
    protocol Sequence {
        associatedtype Iterator : IteratorProtocol
    }
    protocol IteratorProtocol { ... }
    ```


- <i><span style="color: #C0C0C0">**Compensate for weak type information** to clarify a parameter’s role.</span></i>  
- 매개변수의 역할을 명확하게 하기 위해 **약한 타입 정보를 보완합니다.**  

    <i><span style="color: #C0C0C0">Especially when a parameter type is NSObject, Any, AnyObject, or a fundamental type such Int or String, type information and context at the point of use may not fully convey intent. In this example, the declaration may be clear, but the use site is vague.</span></i>  
    특히 매개변수 타입이 NSObject, Any, AnyObject, 또는 Int나 String같은 기본 타입인 경우, 사용 시점에 타입 정보와 콘텍스트가 의도를 완전히 전달하지 못할 수 있습니다. 이 예제에서 선언은 명확할 수 있지만, 사용 장소가 모호합니다.  

    ```swift
    ❌
    func add(_ observer: NSObject, for keyPath: String)

    grid.add(self, for: graphics) // vague
    ```

    <i><span style="color: #C0C0C0">To restore clarity, **precede each weakly typed parameter with a noun describing its role:**</span></i>  
    명확하게 하려면, **각 약한 타입의 매개변수 앞에 그 역할을 설명하는 명사를 위치시킵니다:**  

    ```swift
    ✅
    func addObserver(_ observer: NSObject, forKeyPath path: String)
    grid.addObserver(self, forKeyPath: graphics) // clear
    ```

<br>

## Strive for Fluent Usage

- <i><span style="color: #C0C0C0">**Prefer method and function names that make use sites form grammatical English phrases.**</span></i>

    ```swift
    ✅
    x.insert(y, at: z)          “x, insert y at z”
    x.subViews(havingColor: y)  “x's subviews having color y”
    x.capitalizingNouns()       “x, capitalizing nouns”
    ```

    ```swift
    ❌
    x.insert(y, position: z)
    x.subViews(color: y)
    x.nounCapitalize()
    ```

    <i><span style="color: #C0C0C0">It is acceptable for fluency to degrade after the first argument or two when those arguments are not central to the call’s meaning:</span></i>

    ```swift
    AudioUnit.instantiate(
        with: description, 
        options: [.inProcess], completionHandler: stopProgressBar)
    ```


- <i><span style="color: #C0C0C0">**Begin names of factory methods with “make”,** e.g. x.makeIterator().</span></i>

- <i><span style="color: #C0C0C0">The first argument to **initializer and [factory methods](https://en.wikipedia.org/wiki/Factory_method_pattern) calls** should not form a phrase starting with the base name, e.g. x.makeWidget(cogCount: 47)</span></i>

    <i><span style="color: #C0C0C0">For example, the first arguments to these calls do not read as part of the same phrase as the base name:</span></i>

    ```swift
    ✅
    let foreground = Color(red: 32, green: 64, blue: 128)
    let newPart = factory.makeWidget(gears: 42, spindles: 14)
    let ref = Link(target: destination)
    ```

    <i><span style="color: #C0C0C0">In the following, the API author has tried to create grammatical continuity with the first argument.</span></i>

    ```swift
    ❌
    let foreground = Color(havingRGBValuesRed: 32, green: 64, andBlue: 128)
    let newPart = factory.makeWidget(havingGearCount: 42, andSpindleCount: 14)
    let ref = Link(to: destination)
    ```

    <i><span style="color: #C0C0C0">In practice, this guideline along with those for argument labels means the first argument will have a label unless the call is performing a value preserving type conversion.</span></i>

    ```swift
    let rgbForeground = RGBColor(cmykForeground)
    ```

- <i><span style="color: #C0C0C0">**Name functions and methods according to their side-effects**</span></i>

    - <i><span style="color: #C0C0C0">Those without side-effects should read as noun phrases, e.g. x.distance(to: y), i.successor().</span></i>

    - <i><span style="color: #C0C0C0">Those with side-effects should read as imperative verb phrases, e.g., print(x), x.sort(), x.append(y).</span></i>

    - <i><span style="color: #C0C0C0">**Name Mutating/nonmutating method pairs** consistently. A mutating method will often have a nonmutating variant with similar semantics, but that returns a new value rather than updating an instance in-place.</span></i>

        - <i><span style="color: #C0C0C0">When the operation is **naturally described by a verb,** use the verb’s imperative for the mutating method and apply the “ed” or “ing” suffix to name its nonmutating counterpart.</span></i>   
            
            <img width="800" alt="mutatingNonmutationTable" src="https://user-images.githubusercontent.com/65153742/157592746-1a6f46a9-7fe8-44c3-b27f-858429c3a43a.png">
            - <i><span style="color: #C0C0C0">Prefer to name the nonmutating variant using the verb’s past participle (usually appending “ed”):</span></i>

                ```swift
                /// Reverses `self` in-place.
                mutating func reverse()

                /// Returns a reversed copy of `self`.
                func reversed() -> Self
                ...
                x.reverse()
                let y = x.reversed()
                ```

            - <i><span style="color: #C0C0C0">When adding “ed” is not grammatical because the verb has a direct object, name the nonmutating variant using the verb’s present participle, by appending “ing.”</span></i>

                ```swift
                /// Strips all the newlines from `self`
                mutating func stripNewlines()

                /// Returns a copy of `self` with all the newlines stripped.
                func strippingNewlines() -> String
                ...
                s.stripNewlines()
                let oneLine = t.strippingNewlines()
            ```

        - <i><span style="color: #C0C0C0">When the operation is **naturally described by a noun,** use the noun for the nonmutating method and apply the “form” prefix to name its mutating counterpart.</span></i>
                
            <img width="800" alt="mutatingNonmutationTable2" src="https://user-images.githubusercontent.com/65153742/157592752-2392b4f3-88a0-4af2-be63-b5c54959f7a7.png">

- <i><span style="color: #C0C0C0">**Uses of Boolean methods and properties should read as assertions about the receiver** when the use is nonmutating, e.g. x.isEmpty, line1.intersects(line2).</span></i>

- <i><span style="color: #C0C0C0">**Protocols that describe what something is should read as nouns** (e.g. Collection).</span></i>

- <i><span style="color: #C0C0C0">**Protocols that describe a capability should be named using the suffixes** able, ible, **or** ing (e.g. Equatable, ProgressReporting).</span></i>

- <i><span style="color: #C0C0C0">The names of other **types, properties, variables, and constants should read as nouns.**</span></i>

<br>

## Use Terminology Well

<i><span style="color: #C0C0C0">**Term of Art**</span></i>     
<i><span style="color: #C0C0C0">noun - a word or phrase that has a precise, specialized meaning within a particular field or profession.</span></i>

- <i><span style="color: #C0C0C0">**Avoid obscure terms** if a more common word conveys meaning just as well. Don’t say “epidermis” if “skin” will serve your purpose. Terms of art are an essential communication tool, but should only be used to capture crucial meaning that would otherwise be lost.</span></i>

- <i><span style="color: #C0C0C0">**Stick to the established meaning** if you do use a term of art.</span></i>

    <i><span style="color: #C0C0C0">The only reason to use a technical term rather than a more common word is that it precisely expresses something that would otherwise be ambiguous or unclear. Therefore, an API should use the term strictly in accordance with its accepted meaning.</span></i>

    - <i><span style="color: #C0C0C0">**Don’t surprise an expert**: anyone already familiar with the term will be surprised and probably angered if we appear to have invented a new meaning for it.</span></i>

    - <i><span style="color: #C0C0C0">**Don’t confuse a beginner**: anyone trying to learn the term is likely to do a web search and find its traditional meaning.</span></i>

- <i><span style="color: #C0C0C0">**Avoid abbreviations.** Abbreviations, especially non-standard ones, are effectively terms-of-art, because understanding depends on correctly translating them into their non-abbreviated forms.</span></i>

    > <i><span style="color: #C0C0C0">The intended meaning for any abbreviation you use should be easily found by a web search.</span></i>

- <i><span style="color: #C0C0C0">**Embrace precedent.** Don’t optimize terms for the total beginner at the expense of conformance to existing culture.</span></i>

    <i><span style="color: #C0C0C0">It is better to name a contiguous data structure Array than to use a simplified term such as List, even though a beginner might grasp the meaning of List more easily. Arrays are fundamental in modern computing, so every programmer knows—or will soon learn—what an array is. Use a term that most programmers are familiar with, and their web searches and questions will be rewarded.</span></i>

    <i><span style="color: #C0C0C0">Within a particular programming domain, such as mathematics, a widely precedented term such as sin(x) is preferable to an explanatory phrase such as verticalPositionOnUnitCircleAtOriginOfEndOfRadiusWithAngle(x). Note that in this case, precedent outweighs the guideline to avoid abbreviations: although the complete word is sine, “sin(x)” has been in common use among programmers for decades, and among mathematicians for centuries.</span></i>
