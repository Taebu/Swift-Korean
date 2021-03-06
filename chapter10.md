# 10 열거형 (Enumerations)
> Translator : inureyes (inureyes@gmail.com)

열거형 _(Enumeration)_ 은 관련있는 값들의 그룹에 대한 일반적인 타입을 정의하며, 이를 이용하여 코드 안에서 타입에 안전한 방법으로 작업할 수 있습니다. C에 익숙한 사용자라면, C 열거형은 관련있는 이름을 정수값의 집합(set)에 할당하는 것을 알고 있을 것입니다. Swift의 열거형은 훨씬 더 유연하며, 열거형의 각 숫자마다 반드시 값을 제공할 필요가 없습니다. 만약 ("원시(raw)" 값으로 알려진) 값이 각 열거형 번호마다 제공될 경우, 그 값들은 문자열, 글자, 어떠한 정수나 부동 소수점 타입이 될 수 있습니다. 

또한, 열거형 멤버들은 각각 다른 멤버 값에 대하여 다른 언어의 공용체(union)및 비슷한 기능들이 하듯 연관된 값들을 어떤 타입이든 지정할 수 있습니다. 관련있는 멤버들의 일반적인 집합을 하나의 열거형의 부분으로 정의할 수도 있으며, 각각은 그에 연관된 적당한 타입의 값들의 다양한 집합을 가질 수 있습니다.

Swift의 열거형은 열거형의 현재 값에 대한 추가적인 정보를 제공하기 위한 계산된 프로퍼티나, 열거형이 표현하는 값들과 연관된 기능들을 제공하는 인스턴스 메소드 같이 전통적으로 클래스 등에서만 지원되는 많은 기능들을 차용하였습니다. 또한 열거형은 초기 멤버 값을 제공하는 이니셜라이저(initiailizer)를 제공할 수 있고, 원래 구현을 넘어서 기능을 확장할 수도 있으며, 표준 기능을 제공하기 위한 프로토콜을 따를 수 있습니다.

이러한 기능에 대한 자세한 내용은 [속성](), [메소드](), [초기화](), [확장](), 및 [프로토콜]()을 참조하십시오.


### 열거형 문법 (Enumeration Syntax)
열거형은 `enum` 키워드로 작성하며, 중괄호 안에 모든 정의를 집어넣습니다.
```swift
enum SomeEnumeration {
    // enumeration definition goes here
}
```
여기에 나침반의 4가지 주요 방향을 위한 예제가 하나 있습니다:
```swift
enum CompassPoint {
    case North
    case South
    case East
    case West
}
```
(`North`, `South`, `East` 및 `West` 같이) 열거형에 정의된 값들은 이 열거형의 멤버 값들입니다. `case` 키워드는 멤버 값들의 새 줄이 정의될 것임을 나타냅니다.

>NOTE
C 및 Objective-C 와는 다르게, Swift의 열거형 멤버들은 생성시 기본 정수값들에 할당되지 않습니다. 위의 **CompassPoints** 예제에서 보듯, **North**, **South**, **East** 및 **West**는 명시적으로 **0**, **1**, **2** 및 **3**에 대응되지 않습니다. 대신에, 기본 열거형 멤버들은 **CompassPoint**의 명시적으로 정의된 타입과 함께 정의된 완벽하게 갖춰진 값입니다.

여러 멤버 값들이 콤마(,) 로 구분되어 한 줄에 나올 수도 있습니다:
```swift
enum Planet {
    case Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
}
```
각 열거형 정의들은 새로운 타입을 정의합니다. Swift의 다른 타입과 마찬가지로, 이름들 ( **CompassPoint** 및 **Planet**과 같은) 은 대문자로 시작해야 합니다. 자명하게 읽힐 수 있도록 열거형 타입에게 복수형 대신 단수형 이름을 주세요.
```swift
var directionToHead = CompassPoint.West
```
**directionToHead** 타입은 **CompassPoint**의 가능한 값들 중 하나가 초기화 될 때 유추됩니다. **directionToHead**가 **CompassPoint**로 선언되면, 짧은 닷 구문을 사용하여 그 값을 다른 **CompassPoint** 값으로 할당할 수 있습니다:
```swift
directionToHead = .East
```
**directionToHead**의 타입은 이미 알려져 있으므로, 값을 설정할 때 타입을 명기하지 않을 수 있습니다. 이러한 부분은 명시적으로 타입된 열거형 값들로 작업할 때 매우 읽기 편한 코드를 만들어줍니다.


## 열거형의 값들과 스위치 구문간의 대응 (Matching Enumeration Values with a Switch Statement)

각각의 열거형 값들을 `switch` 구문과 대응할 수 있습니다.
```swift
directionToHead = .South
switch directionToHead {
case .North:
    println("Lots of planets have a north")
case .South:
    println("Watch out for penguins")
case .East:
    println("Where the sun rises")
case .West:
    println("Where the skies are blue")
}
// prints "Watch out for penguins"
```
이 코드는 다음과 같이 읽을 수 있습니다:
"**directionToHead**의 값을 봅시다. 만약 **.North**와 값이 같다면,  **"Lots of planets have a north"** 를 출력합니다. 만약 **.South**와 값이 같다면, **"Watch out for penguins"** 를 출력합니다."

...식이 됩니다.

[제어 구문]() 에서 설명했듯이, `switch` 구문은 열거형 멤버를 고려할때 완벽하게 작성되어야 합니다. 만약 **.West**를 표현하기 위한 `case`가 빠진 경우, 이 코드는 **CompassPoint** 멤버의 완벽한 리스트를 고려하지 않았기 때문에 컴파일되지 않을 것입니다. 완벽성 (exhaustiveness) 의 요구는 열거형 멤버가 실수로 생략되는 것을 방지합니다. 

모든 열거형 멤버에 대한 케이스를 제공하기에 적당하지 않은 경우, 명시적으로 언급되지 않은 멤버들을 위한 기본 케이스를 제공할 수 있습니다.
```swift
    let somePlanet = Planet.Earth
    switch somePlanet {
    case .Earth:
        println("Mostly harmless")
    default:
        println("Not a safe place for humans")
    }
    // prints "Mostly harmless”
```
## 관련된 값들 (Associated Values)

앞 섹션의 예제는 열거형의 멤버들이 각각의 어떻게 정의되었는지 보여줍니다. 상수 및 변수를 **Planet.Earth** 에 할당할 수 있으며, 나중에 값들을 확인할 수도 있습니다. 그렇지만, 종종 멤버 값들과 함께 연관된 다른 타입의 값들을 저장하는 것이 유용한 경우들이 있습니다. 이는 추가적인 사용자 지정 정보를 멤버 값들마다 저장할수 있게 하며, 코드 안에서 멤버를 사용할 때 마다 정보가 변경되는 것을 허용합니다. 

어떤 특정한 타입의 관련 값을 저장하는 Swift 열거형을 정의 할 수 있으며, 필요한 경우에 열거형의 각 멤버에 따라 값의 형식은 다를 수 있습니다. 이러한 열거형과 유사한 경우들이 다른 언어에서는 차별된 공용체 (discriminated union), 태깅된 공용체 (tagged unions) 및 변형체 (variants) 로 알려져 있습니다.

예를 들어 재고 추적 시스템이 각 제품을 두가지 타입의 바코드로 추적할 필요가 있다고 해 봅시다. 어떤 제품들은 UPC-A 포맷의 **0**에서 **9** 사이의 숫자를 사용하는 1차원 바코드로 레이블링 되어 있습니다. 각 바코드는 열 개의 "확인 번호(identifier)" 숫자가 뒤따르는 "번호 시스템" 숫자를 갖고 있습니다. 이 숫자들 뒤에는 각 코드가 제대로 스캔되었는지를 검증하기 위한 "확인(check)" 숫자가 붙습니다.

![chapter10-fig1.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/chapter10-fig1.png)

다른 제품들은 모든 ISO 8859-1 문자를 사용할 수 있으며 2,953글자의 길이를 갖는 QR 코드 포맷의 2차원 바코드로 레이블링되어 있습니다.

![chapter10-fig2.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/chapter10-fig2.png)

재고추적 시스템이 UPC-A 바코드를 3개의 숫자 튜플로 저장하고, QR 코드는 임의의 길이의 문자열로 저장할 수 있다면 매우 편할 것입니다.

Swift에서, 각 유형의 제품의 바코드를 정의하는 열거형은 다음처럼 보일 것입니다:
```swift
enum Barcode {
        case UPCA(Int, Int, Int)
        case QRCode(String)
    }
```
이 코드는 다음과 같이 읽을 수 있습니다:

"`(Int, Int, Int)` 타입의 **UPCA** 값 또는 `String` 타입의 **QRCode** 값을 가질 수 있는 **Barcode**라는 열거형 타입을 정의합니다."

이 정의는 어떠한 실제 `Int` 및 `String` 값을 제공하지 않습니다. 오직 바코드 상수 및 변수들이 **Barcode.UPCA** 또는 **Barcode.QRCode** 중 하나와 같을 때, 그와 연관된 값들의 타입만을 정의합니다. 

이제 새 바코드는 두가지 타입 중 하나로 생성될 수 있습니다:
```swift
var productBarcode = Barcode.UPCA(8, 85909_51226, 3)
```
이 예제는 **productBarcode** 라는 새 변수를 생성하고, **Barcode.UPCA** 의 값으로 **(8, 8590951226, 3)** 튜플 값을 배정합니다. 제공된 "식별자" 값은 바코드로 읽기 좋도록 정수 표현 안의 밑줄 -**85909_51226**-로 을 갖고 있습니다.

동일한 제품이 다른 형태의 바코드로 배정될 수도 있습니다.
```swift
		productBarcode = .QRCode("ABCDEFGHIJKLMNOP")”
```
이 경우, 원래 **Barcode.UPCA** 및 정수 값은 새로운 **Barcode.QRCode** 와 문자열 값으로 대체됩니다. **Barcode** 타입의 상수 및 변수들은 **.UPCA** 또는 **.QRCode** 중 하나를 (해당되는 값들과 함께) 저장할 수 있지만, 한번에 둘 중 하나만 저장할 수 있습니다.

서로 다른 바코드 타입들은 앞에서와 같이 `switch` 구문을 사용하여 체크할 수 있습니다. 그러나, 이번 경우 관련된 값들은 스위치 구분의 일부로 추출될 수 있습니다. 각각의 연관 값들을 `switch`의 `case` 내용으로 사용하기 위하여 (`let` 접두사와 함께) 상수 또는 (`var` 접두사와 함께) 변수로 추출할 수 있습니다. 
```swift
    switch productBarcode {
    case .UPCA(let numberSystem, let identifier, let check):
        println("UPC-A with value of \(numberSystem), \(identifier), \(check).")
    case .QRCode(let productCode):
        println("QR code with value of \(productCode).")
    }
    // prints "QR code with value of ABCDEFGHIJKLMNOP."
```
만약 열거형 멤버들의 모든 연관 값들이 상수로 추출되었거나 모두 변수로 추출되었다면, 간결함을 위하여 멤버 이름 앞에 하나의 `var` 또는 `let` 을 붙일 수 있습니다:
```swift
    switch productBarcode {
    case let .UPCA(numberSystem, identifier, check):
        println("UPC-A with value of \(numberSystem), \(identifier), \(check).")
    case let .QRCode(productCode):
        println("QR code with value of \(productCode).")
    }
    // prints "QR code with value of ABCDEFGHIJKLMNOP."
```
‌
##  원시 값 (Raw Values)
연관값들을 사용한 바코드 예제는 어떻게 열거형의 멤버들이 그들이 저장하는 여러 타입의 관련된 값들을 선언하는지에 대해 보여주었습니다. 연관 값들에 대한 다른 방법으로, 열거형 멤버들은 (원시 값들이라고 부르는) 모두 같은 타입인 기본값들로 미리 채워질 수 있습니다.

아래는 원시 ASCII 값들을 이름붙은 열거형 멤버들에 저장하는 예입니다.
```swift
    enum ASCIIControlCharacter: Character {
        case Tab = "\t"
        case LineFeed = "\n"
        case CarriageReturn = "\r"
    }
```
여기서 **ASCIIControlCharacter** 열거형을 위한 원시 값들은 `Character` 타입이 되도록 정의되었으며, 더 일반적인 ASCII 제어 문자들로 할당되었습니다. `Character` 값들은 [문자열 및 글자]() 에 설명되어 있습니다.

원시 값들은 연관된 값들과 같지 않음을 유의하세요. 원시 값들은 위의 세가지 ASCII 코드들처럼 코드 안에서 처음 열거형을 정의할 때 미리 정의된 값들입니다. 개개의 열거형 멤버들의 원시 값은 언제나 동일합니다. 연관 값들은 새 상수 또는 변수를 열거형의 멤버 중 하나에 기초하여 생성할 때 할당되며, 무엇을 하느냐에 따라 매번 다를 수 있습니다.

원시 값은 문자열, 글자, 정수 또는 어떠한 부동 소수점 타입이 될 수 있습니다. 각각의 원시 값은 열거형 정의 안에서 반드시 유일해야 합니다. 원시 값으로 정수가 사용되었다면, 열거형 멤버의 일부에 아무 값도 설정되지 않은 경우 자동 증가(Auto-incrementation)할 것입니다.

아래의 열거형은 태양으로부터의 순서를 원시 정수값으로 표현하는 **Planet** 열거형의 개선된 형태입니다.
```swift
    enum Planet: Int {
        case Mercury = 1, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
    }
```
자동 증가는 **Planet.Venus**가 2의 원시 값을 갖는 식으로 진행되는 것을 의미합니다.


열거형 멤버의 원시 값들을 **toRaw** 메소드로 읽읍시다:
```swift
    let earthsOrder = Planet.Earth.toRaw()
    // earthsOrder is 3
```

열거형의 **fromRaw** 메소드를 사용하여 특정한 원시 값에 해당되는 열거형 멤버를 찾읍시다. 이 예제는 원시값 **7**에 해당되는 행성이 Uranus임을 판별합니다:
```swift
    let possiblePlanet = Planet.fromRaw(7)
    // possiblePlanet is of type Planet? and equals Planet.Uranus
```
모든 `Int` 값들이 해당되는 행성을 찾을 수 있는 것은 아닙니다. 그러므로, **fromRaw** 메소드는 추가적인 열거형 멤버를 반환합니다. 위의 예에서, **possiblePlanet** 은 **Planet?** 타입이거나 "**optional Planet**" 타입입니다.

만약 9번째 위치에 있는 행성을 찾는다면, **fromRaw**가 반환하는 추가적 **Planet** 값은 `nil` 이 될 것입니다:
```swift
    let positionToFind = 9
    if let somePlanet = Planet.fromRaw(positionToFind) {
        switch somePlanet {
        case .Earth:
            println("Mostly harmless")
        default:
            println("Not a safe place for humans")
        }
    } else {
        println("There isn't a planet at position \(positionToFind)")
    }
    // prints "There isn't a planet at position 9"
```
이 예제는 **9**의 원시값에 해당되는 행성을 읽기 위해 추가적인 바인딩을 사용합니다.
**if let somePlanet = Planet.fromRaw(9)** 는 추가적인 **Planet**을 찾아내고, 찾을 수 있는 경우 추가적인 **Planet**의 내용을 **somePlanet**에 할당합니다. 이 경우, **9**의 위치에 있는 행성을 찾는 것은 불가능하기 때문에 `else` 브렌치가 대신 실행됩니다.
