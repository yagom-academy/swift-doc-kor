
# Special Instructions(특별 지침들)

- <i><span style="color: #C0C0C0">**Label tuple members and name closure parameters** where they appear in your API.</span></i>

    <i><span style="color: #C0C0C0">These names have explanatory power, can be referenced from documentation comments, and provide expressive access to tuple members.</span></i>
    
- API에 튜플과 클로저가 드러나는 곳에서, **튜플의 멤버들에 라벨을 붙여주고 클로저의 매개변수들에 이름을 지어줍시다.**

    이러한 이름들은 설명력을 갖습니다. 이는 문서 주석들에 참조될 수 있고, 튜플의 구성요소들에 대해서 유의미한 접근을 제공합니다.
    
    ```swift
    /// Ensure that we hold uniquely-referenced storage for at least
    /// `requestedCapacity` elements.
    /// 최소한 'requestedCapacity'만큼 유일한 참조 storage를 가지고 있다는 걸 확인합니다.
    ///
    /// If more storage is needed, `allocate` is called with
    /// `byteCount` equal to the number of maximally-aligned
    /// bytes to allocate.
    /// 만약 더 많은 storage가 필요하다면,
    /// 할당을 위해 최대로 조정된 bytes 갯수인 'byteCount'와 함께
    /// 'allocate'가 호출됩니다
    ///
    /// - Returns:
    ///   - reallocated: `true` iff a new block of memory
    ///     was allocated.
    ///   - capacityChanged: `true` iff `capacity` was updated.
    /// - 반환값들:
    ///   - reallocated: 새로운 메모리 블록이 할당된 경우 `true` 
    ///   - capacityChanged: `capacity`가 갱신된 경우 `true`
    mutating func ensureUniqueStorage(
        minimumCapacity requestedCapacity: Int, 
        allocate: (_ byteCount: Int) -> UnsafePointer<Void>
    ) -> (reallocated: Bool, capacityChanged: Bool)
    ```

    <i><span style="color: #C0C0C0">Names used for closure parameters should be chosen like `parameter names` for top-level functions. Labels for closure arguments that appear at the call site are not supported.</span></i>
    
    클로저의 매개변수에서 사용된 것과 같은 이름들은, 최상위 함수들을 위해서 `매개변수 이름들`처럼 사용해야 합니다. 호출부에서 표현되는 클로저의 인자들에 대한 라벨들은 대상이 아닙니다.    

- <i><span style="color: #C0C0C0">**Take extra care with unconstrained polymorphism** (e.g. Any, AnyObject, and unconstrained generic parameters) to avoid ambiguities in overload sets.</span></i>

    <i><span style="color: #C0C0C0">For example, consider this overload set:</span></i>

- overload 집합들에서 애매함을 피하기 위해, **제한사항이 없는 다형성을 특별히 신경씁시다** (예를 들면 Any, AnyObject, 제한사항이 없는 제네릭 매개변수들)

    예를 들어 아래의 overload 집합들을 고려해봅시다.

    ```swift
    ❌
    struct Array {
        /// Inserts `newElement` at `self.endIndex`.
        // `self.endIndex`에 newElement를 넣는다
        public mutating func append(_ newElement: Element)

        /// Inserts the contents of `newElements`, in order, at
        /// `self.endIndex`.
        // `self.endIndex`에 newElement의 구성요소를 순서대로 넣는다
        public mutating func append(_ newElements: S)
            where S.Generator.Element == Element
    }
    ```

    <i><span style="color: #C0C0C0">These methods form a semantic family, and the argument types appear at first to be sharply distinct. However, when Element is Any, a single element can have the same type as a sequence of elements.</span></i>
    
    위 메서드들은 하나의 의미론적 집단을 형성합니다. 이어서 나타나는 첫번째 인자의 타입들은 분명하게 구별됩니다. 하지만 Element가 Any라면, 단일요소가 요소들의 시퀀스와 같은 타입을 가질 수 있습니다.
    
    ```swift
    ❌
    var values: [Any] = [1, "a"]
    values.append([2, 3, 4]) // [1, "a", [2, 3, 4]] or [1, "a", 2, 3, 4]?
    ```

    <i><span style="color: #C0C0C0">To eliminate the ambiguity, name the second overload more explicitly.</span></i>
    
    모호함을 없애기 위해서, 두번째 overload는 보다 분명하게 이름을 지어야합니다

    ```swift
    ✅
    struct Array {
        /// Inserts `newElement` at `self.endIndex`.
        // `self.endIndex`에 newElement를 넣는다
        public mutating func append(_ newElement: Element)

        /// Inserts the contents of `newElements`, in order, at
        /// `self.endIndex`.
        // `self.endIndex`에 newElement의 구성요소를 순서대로 넣는다
        public mutating func append(contentsOf newElements: S)
            where S.Generator.Element == Element
    }
    ```

    <i><span style="color: #C0C0C0">Notice how the new name better matches the documentation comment. In this case, the act of writing the documentation comment actually brought the issue to the API author’s attention.</span><i>
    새로운 이름과 문서 주석이 얼마나 더 잘 어울리는지 주목해주세요. 이처럼, 문서 주석을 적는 행위는 실제로 API 작성자의 이목을 끌게 됩니다.
    
    
