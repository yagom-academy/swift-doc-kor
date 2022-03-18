
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

## Use Terminology Well(적절한 기술 용어 사용하기)

<i><span style="color: #C0C0C0">**Term of Art**</span></i>
**기술 용어**     
<i><span style="color: #C0C0C0">noun - a word or phrase that has a precise, specialized meaning within a particular field or profession.</span></i>
명사 - 특정 분야나 직군 내에서 명확하고 전문적인 의미를 가진 하나의 단어 혹은 문구

- <i><span style="color: #C0C0C0">**Avoid obscure terms** if a more common word conveys meaning just as well. Don’t say “epidermis” if “skin” will serve your purpose. Terms of art are an essential communication tool, but should only be used to capture crucial meaning that would otherwise be lost.</span></i>
- 만약 일반적으로 사용되는 단어로도 충분히 의미가 전달된다면 **이해하기 힘든 용어는 사용하지 않도록 합니다.** "피부"라는 단어만으로도 의도가 충분히 전달된다면 "표피"라는 단어로 표현하지 않도록 합니다. 기술 용어는 중요한 소통 수단이지만, 이를 사용하지 않고서는 용어가 가진 중요한 의미를 담을 수 없을 경우에만 사용해야 합니다.

- <i><span style="color: #C0C0C0">**Stick to the established meaning** if you do use a term of art.</span></i>
- 만약 기술 용어를 사용한다면 **용어가 가지고 있는 의미에 충실하도록 사용해야 합니다.**

    <i><span style="color: #C0C0C0">The only reason to use a technical term rather than a more common word is that it precisely expresses something that would otherwise be ambiguous or unclear. Therefore, an API should use the term strictly in accordance with its accepted meaning.</span></i>
    다른 용어를 사용함으로써 그 의미가 모호해지고 불명확해질 경우 이를 명확하게 하고자할 때만 기술 용어를 사용해야 합니다. 따라서, API에서 사용되는 용어는 그 의미가 어떻게 받아들여질지를 고려해서 엄격하게 선별하여 사용해야 합니다.

    - <i><span style="color: #C0C0C0">**Don’t surprise an expert**: anyone already familiar with the term will be surprised and probably angered if we appear to have invented a new meaning for it.</span></i>
    - **전문가가 경악하지 않도록 합니다**: 만약 용어를 기존의 의미가 아닌 쓰이지 않던 새로운 의미로써 사용한다면 그 용어에 대해 이미 익숙한 사람은 경악하고 불쾌해 할 것입니다.
    
    - <i><span style="color: #C0C0C0">**Don’t confuse a beginner**: anyone trying to learn the term is likely to do a web search and find its traditional meaning.</span></i>
    - **초심자 입장에서 혼동되지 않도록 합니다**: 누군가는 웹 검색을 통해 사용된 용어가 가지고 있는 고유한 의미를 찾아내려고 할 것입니다.  

- <i><span style="color: #C0C0C0">**Avoid abbreviations.** Abbreviations, especially non-standard ones, are effectively terms-of-art, because understanding depends on correctly translating them into their non-abbreviated forms.</span></i>
- **축약어를 피하도록 합니다.** 표준 축약어가 아닌 축약어들은 실질적으로 기술 용어입니다. 축약되지 않은 표현으로 얼마나 잘 번역하는지에 따라 그 기술 용어에 대한 이해도가 달라지기 때문입니다.(따라서 누구나 쉽게 이해할 수 있도록 축약어를 피하는 것이 좋습니다.)

    > <i><span style="color: #C0C0C0">The intended meaning for any abbreviation you use should be easily found by a web search.</span></i>
    > 축약어를 사용할 때 의도했던 의미를 웹 검색으로 쉽게 찾을 수 있어야 합니다.

- <i><span style="color: #C0C0C0">**Embrace precedent.** Don’t optimize terms for the total beginner at the expense of conformance to existing culture.</span></i>
- **선례를 따르도록 합니다.** 일반적으로 사용해오던 기존의 용어를 대체하면서까지 초심자의 눈높이에 맞춰 용어를 최적화하지 않도록 합니다. 

    <i><span style="color: #C0C0C0">It is better to name a contiguous data structure Array than to use a simplified term such as List, even though a beginner might grasp the meaning of List more easily. Arrays are fundamental in modern computing, so every programmer knows—or will soon learn—what an array is. Use a term that most programmers are familiar with, and their web searches and questions will be rewarded.</span></i>
    연속적인 자료구조의 이름으로 List보다는 Array를 사용하는 것이 좋습니다. List를 사용하는 것이 초심자에게는 그 뜻을 이해하기에 더 쉬울지 모르지만 말입니다. Array는 현대 컴퓨팅의 기초이기 때문에 모든 프로그래머들이 알고 있고, 설령 아직 배우지 않은 프로그래머일지라도 곧 배우게 됩니다. 대부분의 프로그래머들에게 익숙한 용어를 사용하면 웹 검색과 질문을 통해 그 의미를 쉽게 찾을 수 있습니다.

    <i><span style="color: #C0C0C0">Within a particular programming domain, such as mathematics, a widely precedented term such as sin(x) is preferable to an explanatory phrase such as verticalPositionOnUnitCircleAtOriginOfEndOfRadiusWithAngle(x). Note that in this case, precedent outweighs the guideline to avoid abbreviations: although the complete word is sine, “sin(x)” has been in common use among programmers for decades, and among mathematicians for centuries.</span></i>
    수학과 같은 특정 프로그래밍 도메인에서 sin(x)와 같은 용어는 해당 표현의 의미를 풀어서 설명한 verticalPositionOnUnitCircleAtOriginOfEndOfRadiusWithAngle(x)보다 일반적으로 사용되고 있는 sin(x)를 사용하는 것이 좋습니다. sin(x)는 축약어 이지만 이 경우 축약어를 피하라는 권장 지침을 지키는 것 이상으로 기존에 사용해왔던 선례를 따르는 것이 중요하다는 점에 유의하도록 합니다: 비록 정확한 단어는 sine이지만, 수십 년간 프로그래머들과 또 수 세기 동안 수학자들 사이에서는 "sin(x)"라는 표현이 일반적으로 사용되어 왔습니다.
