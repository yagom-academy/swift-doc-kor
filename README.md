
#### 야곰 아카데미에서 제작한 [swift-doc-kor](https://yagom-academy.github.io/swift-doc-kor/) 저장소입니다.

<br>

<img alt="image" src="https://user-images.githubusercontent.com/73867548/156685343-a7dd49af-b52d-40fd-88bc-8c63a2cef135.png">

<br>

# 🐻 야곰 아카데미 공유 문서 참여 방법

안녕하세요. `swift-doc-kor`에 함께 해주셔서 감사합니다.   
`swift-doc-kor`는 누구나 참여할 수 있습니다. 멋진 공유의 장을 만들어보아요! 참여 방법은 아래 가이드를 차근차근 확인해주세요 🙂

<br>


## 📮  필독 사항

- **반드시 [`Wiki`](https://github.com/yagom-academy/swift-doc-kor/wiki/용어-위키) 에서 용어를 참고하여 정확하고 일관된 용어를 사용합니다.**
- `-니다.` 체를 사용합니다.
- 가능하면 번역체가 아닌 한국어 표현으로 번역합니다.
    
    > 🔎 **예시**   
    Swift 코드는 간결할 수 있지만 가장 적은 문자로 가능한 가장 작은 코드를 활성화하는 것은 목표가 아닙니다.    
    ⇒ Swift는 간결하게 사용할 수 있지만, 가능한 가장 적은 문자와 코드를 사용하는 것이 목적은 아닙니다.
    > 
- 마감 기한은 이슈 발행일(코멘트)로부터 **짧은 분량은 7일, 긴 분량의 글은 14일**로 합니다.
    - **짧은 분량**: 페이지의 부분을 담당할 경우 (예시: API Design Guidelines의 한 파트), 문서의 일부분을 수정할 경우
    - **긴 분량**: 한 주제의 글 전체를 담당할 경우 (예시: WWDC 한 편)
- 마감 기한이 지나면 이슈는 Close됩니다.
- 작업 중인 이슈가 있으면 다른 작업에 참여할 수 없습니다.
- 작업 브랜치, 커밋, PR 형식은 안내된 컨벤션을 지키며 작업합니다.

<br>

### ✍🏻 Branch
- 작업할 branch의 이름은  `{Issue Number}-{Nick Name}` 형식을 따릅니다.
    - ex) `#6-odongnamu`
- 작업 완료 후 PR은 작업 내용에 따라 원본 저장소의 아래 브랜치로 요청합니다.
    - API Design Guidelines: `develop-apiGuide`

### ✍🏻 Commit
- commit에는 이슈 번호를 반드시 추가합니다.
- commit 규칙은 아래의 prefix를 따릅니다.
    - add: 새로운 내용을 작성할 때
    - edit: 작성한 내용을 수정할 때
    - chore: 기타 사소한 수정 사항 (띄어쓰기, 줄 바꿈 등)
- ex) `#6 add: {commit 내용}`

<br>

---

<br>

## 1️⃣ Issue 확인 및 만들기

- 먼저 저장소의 Issue를 확인합니다.
- 작성자의 코멘트가 없는 Issue는 주인이 없는 Issue입니다. 주인이 없는 이슈에 코멘트를 달고 작업을 시작합니다.
    - Issue 제목은 `{MileStone}: 작성할 내용 제목` 으로 작성합니다.
    - 코멘트를 달 때에는 템플릿을 복사하여 템플릿의 내용을 필수로 기재합니다.
    - 작업 유형은 `new` / `edit` 중 유형에 따라 골라서 기재합니다.
    - 수정 작업을 할 경우에는 어떤 수정을 할 것인지 구체적으로 기록합니다.
- Issue에 없는 작업을 하고 싶다면 직접 Issue를 생성한 후 작업을 시작합니다.
    
    > 🔎 **예시**
    > - 기존에 번역된 문서에 수정이 필요한 경우
    > - Issue에 없는 새로운 내용(문서)을 작성하고 싶은 경우
    
<br>

<img alt="image" src="https://user-images.githubusercontent.com/73867548/156685406-6962c5d0-7a19-4204-b551-e58cee98a12b.png">

<img alt="image" src="https://user-images.githubusercontent.com/73867548/157362206-82f12623-8792-437a-8e87-78909c9fd927.png">

<br>

## 2️⃣ Fork 후 작업하기

- 이슈를 기록한 후, 저장소를 자신의 저장소로 Fork 합니다. 이미 저장소를 Fork한 상태라면 자신의 저장소로 바로 이동하면 됩니다.
- 로컬에서 작업할 브랜치와 디렉토리 및 파일(필요하다면)을 생성하여 작업합니다.
- 브랜치와 커밋 컨벤션은 반드시 `필독사항의 규칙`을 따라주세요.
- **작업할 때 반드시 [`Wiki`](https://github.com/yagom-academy/swift-doc-kor/wiki/용어-위키) 를 참고하여 정확하고 통일된 용어를 사용합니다.**
- 내가 작성(번역)한 문장을 소리내어 읽어보았을 때 숨이 차거나 부자연스럽다면 문장을 짧게 끊어봐도 좋습니다.
- API Design Guidelines 문서 작업시 [`API Design Guidelines 번역 가이드 문서`](https://github.com/yagom-academy/swift-doc-kor/blob/main/guide/API%20Design%20Guidelines%20번역%20가이드.md) 를 참고해주세요.

<br>

## 3️⃣ PR 보내기

- 본인의 저장소에서 작업이 끝난 후 PR을 보냅니다.
- 본인이 작업한 브랜치를 git book 저장소의 `develop` 브랜치에 PR 요청합니다.
- PR 제목은 `{이슈번호} {MileStone}: 담당한 파트(혹은 문서)` 형식으로 작성합니다.
- PR 내용에는 제공하는 템플릿에 맞춰 작성자, 작업 내용 등을 요약하여 기재합니다.
- 수정 작업을 한 경우 수정 내용을 구체적으로 기록합니다.
- 템플릿의 `Closes {이슈 번호}` 에서 {이슈 번호}에 작업한 이슈 번호를 **정확하게** 적습니다. (Merge될 경우 해당 이슈가 자동으로 Close됩니다.)
    - ex) `Closes #3`
- PR 보내기에 익숙하지 않다면 아래 문서를 참고하시면 됩니다 🙂 **브랜치, 커밋, PR 규칙은 반드시 위의 내용을 따라주세요!**
    - [PR 보내는 방법](https://docs.google.com/document/d/1G4Pjs1Fm2-TuhksPt5gO6ERvsEYLEb0MWHcFhBtE60Y/edit#)

<br>

### 🔎 PR 기본 템플릿

<img alt="image" src="https://user-images.githubusercontent.com/73867548/156685623-811d074b-65ef-4059-b280-dc9151825df3.png">

### 🔎 PR 예시

<img alt="image" src="https://user-images.githubusercontent.com/73867548/156685656-b37b8657-7773-4f55-88c5-c42b575e9dcb.png">
