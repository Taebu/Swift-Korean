# 11 클래스와 구조체 (Classes and Structures)
> Translator : 문대선(daeseonmoon@gmail.com)

클래스와 구조체는 프로그램의 코드블럭을 정의할때 사용됩니다. 당신의 클래스와 구조체에 기능을 더하기 위해서 상수, 변수, 그리고 함수를 정의할때와 동일한 문법으로 프로퍼티와 메서드를 정의할 수 있습니다.

다른 프로그래밍 언어와는 달리, Swift는 사용자 클래스와 구조체(custom classes and structures)를 위해서 인터페이스 파일과 구현 파일을 나누어서 만들 필요가 없습니다. Swift에서는 한 파일에서 클래스나 구조체를 정의하며, 다른 코드에서 사용하기 위한 그 클래스와 구조체의 외부 인터페이스는 자동적으로 생성됩니다.

>NOTE
클래스의 인스턴스는 전통적으로 오브젝트로 알려져 있습니다. 하지만 Swifit의 클래스와 구조체는 다른 언어보다도 기능(functionality)에 가깝고, 이 챕터의 대부분은 클래스나 스트럭쳐 타입의 인스턴스에 적용 가능한 기능을 설명할 것입니다. 이런 이유때문에 일반적인 용어로서의 인스턴스가 사용될 것입니다.

## 클래스와 구조체의 비교 (Comparing Classes and Structures)
Swift에서 클래스와 구조체는 여러 공통점을 가지고 있습니다. 공통적으로 가능한 것으로는:
 * 값을 저장하기 위한 프로퍼티를 정의할 수 있습니다.
 * 기능을 제공하기 위한 메서드를 정의할 수 있습니다.
 * 서브스크립트 문법을 사용하여 그들의 값에 접근하는 것을 제공하는 서브스크립트들을 정의 할 수 있습니다.
 * 그들의 초기 상태를 설정하기 위한 Initializer를 정의할 수 있습니다.
 * 기본적인 구현을 넘어서 그들의 기능을 확장시키기 위한 확장(expand)이 가능합니다.
 * 특정 종류의 표준 기능을 제공하는 것으로 프로토콜을 따를 수 있습니다.

더 많은 정보를 원하신다면 [Properties](), [Methods](), [Subscripts](), [Initialization](), [Extensions]() 그리고 [Protocols]() 항목을 참조하십시오.

클래스는 구조체는 할 수 없는 다음과 같은 추가적인 기능들을 지원합니다 : 
 * 상속은 어느 클래스가 다른 클래스의 특성을 상속받을 수 있게합니다.
 * 타입 변환(TypeCasting)은 여러분이 작동시(runtime)에 클래스의 타입을 확인하고 변환을 가능하게합니다.
 * 해제(Deinitializer)는 클래스의 인스턴스가 할당된 자원을 환원 가능케합니다.
 * 참조카운팅(Reference counting)은 하나의 클래스 인스턴스를  한번 이상 참조하는 것을 가능하게 합니다.

더 많은 정보를 원하신다면 [Inheritance](), [Type Casting](), [Initialization]() 그리고 [Automatic Reference Counting]() 항목을 참조하십시오.

>NOTE
여러분의 코드에서 구조체를 전달할때, 구조체는 언제나 복사가 될뿐, 참조카운팅을 사용하지 못합니다.

### 정의 문법 (Definition Syntax)
클래스와 구조체는 유사한 문법적 구조를 가지고 있습니다. 클래스는 `class` 키워드를 구조체는 `struct` 키워드를 사용합니다.  구조체와 클래스 모두 그들의 모든 정의는 중괄호({})내에 위치시킵니다.
```swift
class SomeClass {
    // class definition goes here
}
struct SomeStructure {
    // structure definition goes here
}
```
>NOTE
새로운 클래스나 구조체를 정의할때마다 새로운 Swift의 타입을 효과적으로 정의 할 수있다. `String`, `Int`, 그리고 `Bool`와 같은 표준의 Swift타입과 동일한 대문자 사용법과 맞도록 타입들에게 `SomeClass`나 `SomeStructure`와 같은 `UserCamelCase`에 따른 이름을 주십시오. 역으로 프로퍼티와 메서드는 이들과 타입이름으로 구분이 되도록 `frameRate`나 `incrementCount`와 같은 `lowerCamelCase`에 따른 이름을 주십시오.

클래스와 구조체 정의문의 예:
```swift
struct Resolution {
	var width = 0
	var height = 0
}

class VideoMode {
	var resolution = Resolution()
	var interlaced = false
	var frameRate = 0.0
	var name: String?
}
```
위의 예제는 픽셀기반 해상도를 표현하기 위한 `Resolution`이란 새로운 구조체를 정의합니다. 이 구조체는 `width`와 `height`라는 두개의 저장된 프로퍼티(stored property)를 가지고 있습니다. 저장된 프로퍼티는 클래스의 변수나 상수로서 구성되고 저장된 변수나 상수입니다. 이 두 프로퍼티는 정수값 0으로 초기화된 `int`타입으로 표현됩니다.

위의 예제는 또한 비디오 화면을 위한 특정 비디오 모드를 정의하는 `VideoMode`라는 클래스를 정의합니다. 이 클래스는 네개의 변수인 저장된 프로퍼티를 가지고 있습니다. 첫번째로 `resolution`은 새로운 `Resolution`구조체의 인스턴스로 초기화됩니다. 즉 `Resolution`의 프로퍼티 타입으로 표현됩니다. 나머지 세개의 프로퍼티들은, 새로운 `VideoMode`인스턴스들은 각각 
`interanced`는 non-interlaced 비디오라는 의미의 `false`로 초기화 되고, 재생시 frame Rate는 0.0으로 초기화 된다. 그리고 `name`이라 불리는 옵셔널 `String`값이 있다. `name` 프로퍼티는 옵셔널 타입이기 때문에 자동적으로 "`name` 프로퍼티에 값이 없다"는 의미인 `nil`로 기본값이 주어집니다.

### 클래스와 구조체 인스턴스 (Class and Structure Instances)
`Resolution` 구조체와 `VideoMode` 클래스는 오직 `Resolution`또는 `VideoMode`가 어떻게 보일지를 정의할뿐, 특정한 해상도나 비디오 모드를 표현하지는 않습니다. 그러기에, 여러분은 구조체나 클래스의 인스턴스를 만들 필요가 있습니다.

구조체나 클래스 인스턴스를 생성하기 위한 문법은 매우 유사합니다:
```swift
let someResolution = Resolution()
let someVideoMode = VideoMode()
```
구조체와 클래스는 둘 다 새 인스턴스를 생성하기위해 Initializer 문법을 사용합니다. 가장 간단한 형태의 Initializer 문법은 `Resolution()`이나 `VideoMode()`와 같이 클래스나 구조체의 타입 이름에 빈 괄호(())를 덧붙인 것을 사용하는 것입니다. 이는 각 프로퍼티가 기본값으로 초기화 되어 있는 클래스나 구조체의 새 인스턴스를 생성합니다. 자세한 클래스와 구조체의 초기화는 [Initialization]() 항목을 참조하십시오.

### 프로퍼티 접근 (Accessing Properties)
dot(.) 문법을 사용해서 인스턴스의 프로퍼티에 접근할 수 있습니다. dot 문법에서,  인스턴스 이름 뒤에 아무런 공간 없이 바로 dot(.)과 프로퍼티 네임을 적는것입니다.
```swift
println("The width of someResolution is \(someResolution.width)")
// prints "The width of someResolution is 0"
```
이 예제에서 `someResolution.width`는 `someResolution`의 `width` 프로퍼티를 참조하고 기본 초기값인 0를 반환합니다.

여러분은 원하는 정보를 찾기 위해 내부 프로퍼티로 계속 들어갈 수 있습니다. 예를 들면 `VideoMode`에 속한 `resolution` 프로퍼티내의 `width` 프로퍼티와 같이 말입니다.
```swift
println("The width of someVideoMode is \(someVideoMode.resolution.width)")
// prints "The width of someVideoMode is 0"
```
dot 문법을 통해 변수 프로퍼티로서 새로운 값을 할당하는것도 가능합니다.
```swift
someVideoMode.resolution.width = 1280
println("The width of someVideoMode is now \(someVideoMode.resolution.width)")
// prints "The width of someVideoMode is now 1280"
```
> NOTE
Objective-C와는 달리 Swift는 구조체 프로퍼티의 내부프로퍼티들을 직접적으로 설정하는 것이 가능합니다. 위의 마지막 예제를 보면, `someVideoMode`의 `resulotion` 프로퍼티내의 `width` 프로퍼티의 값을 `resolution` 프로퍼티의 전체를 새로운 값으로 설정 할 필요없이 직접적으로 설정하고 있습니다.

### 구조체 타입을 위한 멤버들의 초기화 (Memberwise Initializers for Structure Types)
모든 구조체는 여러분이 새로은 구조체 인스턴스의 멤버 프로퍼티들을 초기화 할수있는 자동 생성된 멤버들의 initializer(memberwise initializer) 가지고 있습니다. 새로운 인스턴스의 프로퍼티들을 위한 초기값들은 이름을 통해서 멤버들의 initializer에게 전달 될 수 있습니다.
```swift
let vga = Resolution(width: 640, height: 480)
```
구조체와 다르게, 클래스 인스턴스는 기본 멤버들의 initializer를 받지 않습니다. Initializer의 자세한 사항은 [Initialization]()을 참조해주십시오.

## 구조체와 열거형은 값 타입 (Structures and Enumerations Are Value Types)
값 타입(value type)은 변수나 상수에게 할당될 때나 함수에게 값이 전달될 때, 복사되는 타입입니다.

여러분은 지금까지 전 챕터까지 내내 값 타입을 광범위하게 사용했습니다. 사실 Swift에서 기본 형- 정수, 부동 소숫점수, 이진형, 문자열, 배열과 딕셔너리-은 전부 값형식이고 보이지 않는 곳에서 구조체로 구현되어 있습니다.

Swift에서 모든 구조체와 열거형은 값 타입입니다. 즉 여러분이 생성하는 모든 구조체와 열거형 인스턴스들, -그리고 프로퍼티로서 그들이 가지고 있는 모든 값 타입-은 여러분의 코드내에서 전달되는 경우에는 언제나 복사됩니다.

앞의 예제에서 사용된 예제에서 `Resolution` 구조체의 사용에 대해서 더 생각해보자:
```swift
let hd = Resolution(width: 1920, height: 1080)
var cinema = hd
```
이 예제는 `hd`라는 상수를 선언하고 full HD video(1920 픽셀 넓이에 1080 픽셀 높이)의 넓이와 높이로 초기화된 `Resolution` 인스턴스로 설정하였습니다.

그리고 `cinema`라는 변수를 선언하고 `hd` 상수의 현재 값으로 설정했습니다. `Resolution`이 구조체이기 때문에 존재하는 인스턴스의 복사본이 만들어지고, 이 새로운 복사본이 `cinema`에 할당됩니다. `hd`와 `cinema`가 현재 같은 넓이와 높이 값을 가지고 있다하더라도, 그들은 보이지 않는 곳에서는 완전히 다른 두 개의 인스턴스들입니다.

다음은 `cinema`의 `width` 프로퍼티에 디지털 시네마 프로젝션을 위해 사용되는 slightly-wider 2K 표준값의(2048 픽셀 넓이와 1080 픽셀 높이)의 넓이로 수정합니다.
```swift
cinema.width = 2048
```
`cinema` 인스턴스의 `width` 프로퍼티를 체크하는 것으로 이 값이 정말로 2048로 변했음을 볼 수 있습니다.
```swift
println("cinema is now \(cinema.width) pixels wide")
// prints "cinema is now 2048 pixels wide"
```
하지만 `hd` 인스턴스의 `width` 프로퍼티는 여전히 예전 값인 1920를 가지고 있습니다.
```swift
println("hd is still \(hd.width) pixels wide")
// prints "hd is still 1920 pixels wide"
```
`cinema`에 `hd` 인스턴스를 할당할때 `hd`에 저장되어있던 프로퍼티의 값들이 새로 생성된 `cinema` 인스턴스로 복사가 이루어졌음을 알수 있습니다. 결과를 보면 동일한 값을 가지고 있는 완전히 분리된 인스턴스임을 알수 있습니다. 두 인스턴스는 서로 다른 인스턴스이기 때문에 `cinema`의 `width`를 2048로 할당하더라도 `hd` 인스턴스에 저장되어있는 width 값에는 어떠한 영향도 미치지 않습니다.

열거형에도 동일한 법칙이 적용됩니다
```swift
enum CompassPoint {
	case North, South, East, West
}
var currentDirection = CompassPoint.West
let rememberedDirection = currentDirection
currentDirection = .East
if rememberedDirection == .West {
	println("The remembered direction is still .West")
}
// "The remembered direction is still .West" 출력
```
`rememberedDirection`에 `currentDirection`의 값이 할당될때 그 값의 복사본이 실제로 설정됩니다. 그러므로 `currentDirection`의 값이 변경된후에도 `rememberedDirection`에 저장된 원래 값에 복사본에는 어떠한 영향도 미치지 않습니다.

## 클래스는 참조 타입 (Classes Are Reference Types)
값 타입과 달리 참조 타입(reference type)은 함수로 전달되때나 상수나 변수에 할당될때 복사가 이루어지지 않습니다. 복사본 대신, 동일한 인스턴스의 레퍼런스(reference)가 사용됩니다.

위에서 정의한 `VideoMode` 클래스의 사용을 통한 예제가 있습니다:
```swift
let tenEighty = VideoMode()
tenEighty.resolution = hd
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0
```
이 예제에서 우리는 `tenEighty`라는 상수를 선언하고 새로 생성된 `VideoMode` 클래스의 인스턴스를 할당합니다. 비디오 모드는 전에 설정했던 1920 x 1080의 HD 해상도의 복사본을 할당했습니다. 또한 interlaced를 설정하고 "1080i"라는 이름을 주었습니다. 마지막으로 frame rate는 프레임 레이트를 초당 25.0 프레임으로 설정했습니다.

다음으로 `tenEighty`를 `alsoTenEighty`라는 새로운 상수에 할당하고, `alsoTenEighty`의 프레임 레이트의 값을 수정하겠습니다.
```swift
let alsoTenEighty = tenEighty
asloTenEighty.frameRate = 30.0
```
클래스는 참조 타입이기때문에 `tenEighty`와 `alsoTenEighty`는  사실 동일한 `VideoMode` 인스턴스를 참조하고 있습니다. 실제적으로 그들은 단지 동일한 인스턴스를 참조하는 서로 다른 이름일뿐입니다.

아래의 예제코드를 통해 `tenEighty`의 `framerate` 프로퍼티가 새로운 프레임 레이트 값인 30.0임을 확인할수 있습니다.
```swift
println("The frameRate property of tenEighty is now \(tenEighty.frameRate)")
// prints "The frameRate property of tenEighty is now 30.0"
```
`tenEighty`와 `alsoTenEighty`가 변수가 아니라 상수로 선언되었음을 주의깊게 보십시오. `tenEighty`와 `alsoTenEighty` 상수의 그자체는 실제적으로 바뀌지 않기때문에 여러분은 여전히 `tenEighty.frameRate`과 `alsoTenEighty.frameRate`의 값을 바꿀수 있습니다. 
`tenEighty`와 `alsoTenEighty` 자체는 `VideoMode` 인스턴스를 "저장"하지 않고 보이지 않는 곳에서 `VideoMode` 인스턴스를 참조만 합니다. 바뀌는것은 참조되고 있는 `VideoMode`의 `frameRate`프로퍼티이지 `VideoMode`를 참조하고 있는 상수의 값은 변하지 않습니다.

## 식별연산자(Identity Operators)
클래스는 참조타입이기때문에 여러 상수나 변수가 동일한 클래스의 인스턴스를 참조하는게 가능합니다.(구조체와 열거형은 할당되거나 함수에 매개변수로 전달될때 복사가 이루어지기때문에 동일한 인스턴스의 참조는 불가능합니다.)

이러한 이유로 두 상수나 변수가 정확하게 동일한 클래스의 인스턴스를 참조하고 있는지 알아내는것은 종종 유용하게 사용됩니다. 그러한 경우를 알아내기 위해서 Swift는 아래의 두가지 식별연산자를 제공합니다
* 동일한(Identical to) (===)
* 동일하지 않은(Not identical to) (!==)

두 상수나 변수가 동일한 인스턴스를 가리키는지 검사하기 위해 위의 두 연산자를 사용하십시오.
```swift
if tenEighty === alsoTenEighty {
	println("tenEighty and alsoTenEighty refer to the same Resolution instance.")
}
// "tenEighty and alsoTenEighty refer to the same Resolution instance." 출력
```
"동일한(identical to)"("==="로 표현된)과 "같은(equal to)"("=="로 표현된)가 같지 않다라것에 주의하십시오.
* "동일한"은 클래스 형의 두 상수나 변수가 정확하게 동일한 클래스 인스턴스를 참조하고 있음을 뜻합니다.
* "같은"은 두 인스턴스가 같은 값을 가지고 있는지를 검사합니다.

여러분이 사용자 클래스나 구조체를 정의할때 두 인스턴스가 "같은"조건을 결정하는것은 여러분의 결정입니다.
여러분만의 "같은"과 "같지않은(not equal to)"연사자를 구현하는 과정에 대한 자세한 사항은 `Equivalence Operators`를 참조하십시오.

## 포인터 (Pointers)
만약 여러분이 C나 C++ 또는 Objective-C를 사용해본 경험이 있으시다면 이 언어들이 메모리주소를 참조하기 위해 포인터를 사용한다는 것을 아실겁니다. 어떤 참조형식 인스턴스를 참조하는 Swift 상수나 변수는 C에서의 포인터와 유사합니다. 하지만 이것은 메모리상의 주소를 직접적으로 가르키는 것은 아니고 또한 여러분이 생성한 인스턴스를 가르키기 위해 asterisk(*)를 필요로 하지도 않습니다. 대신 Swift에서는 이러한 레퍼런스들은 다른 상수나 변수처럼 정의할수 있습니다.

## 클래스와 구조체중에 선택하기 (Choosing Between Classes and Structures)
여러분 프로그램 코드의 특정 분리된 블록을 사용자 데이터 형으로 정의하기위해 여러분은 클래스나 구조체를 사용할수 있습니다.

하지만 구조체 인스턴스는 언제나 값을 전달하고 클래스 인스턴스는 참조변수를 전달합니다. 즉 이것은 이들이 서로 다른 종류의 작업에 적합하다는것을 뜻합니다. 여러분은 프로젝트에 필요한 데이터 집합이나 기능을 정의할때 그것들이 클래스로 정의되어야 할지 구조체로 정의되어야 할지 결정해야 한다는걸 생각하십시오.

일반적인 가이드로는 아래의 조건중에 한가지또는 그 이상일 경우에는 구조체를 생각하십시오.
* 구조체의 주목적이 몇몇 연관성있는 간단한 데이터 값의 캡슐화일 경우
* 캡술화된 값들이 그 구조체의 인스턴스가 할당될때나 전달될때 참조보다는 복사가 예상될 경우
* 구조체에 저장되는 모든 프로퍼티들이 참조보다는 복사가 예상되는 값형식일 경우
* 구조체가 다른 형(type)에서부터 프로퍼티나 기능이 상속될 필요가 없을 경우

구조체를 사용하는 좋은 예:
* `Double`형을 갖는 width와 height 프로퍼티의 캡슐화를 하는 기하학적 모형의 사이즈.
* `Int`형을 갖는 start와 length 프로퍼티의 캡슐화를 하는 시리즈의 범위에 접근하는 방법.
* `Double`형을 갖는 x,y와 z 프로퍼티의 캡슈화를 하는 3차원 좌표시스템의 포인터.

이외의 경우에는 클래스로 정의하고 레퍼런스로 전달되고 관리되는 클래스의 인스턴스를 생성하십시오. 실질적으로는 대부분의 사용자 데이터 형은 구조체가 아닌 클래스로 정의되어야 합니다.

## 컬렉션 형의 할당과 복사 (Assignment and Copy Behavior for Collection Types)
Swift의 `Array`와 `Dictionary` 형은 구조체로 구현되어 있습니다. 하지만 배열의 경우에는 다른 구조체가 함수나 메소드에 전달될때나 상수나 변수에 할당될때와는 약간 다르게 복사가 작동합니다.

이후에 설명할 `Array`와 `Dictionary`의 복사는 구조체가 아닌 클래스로 구현된 `NSArray`와 `NSDictionary`의 복사와도 또한 다르게 작동합니다. `NSArray`와 `NSDictionary`인스턴스는 언제나 복사가 아니라 인스턴스의 레퍼런스가 전달되거나 할당됩니다.

> NOTE
밑에 설명은 배열, 딕셔너리, 문자열 그리고 다른 값의 "복제"를 설명합니다. 복제가 언급된곳에서 여러분은 여러분의 코드가 언제나 복사처럼 작동하는것을 보게 될것입니다. 하지만 Swift는 절대적으로 필요할 경우에만 실제 값의 복사가 일어납니다. Swift는 추가적인 성능적 향상을 위해서 모든 값의 복사를 관리합니다. 그리고 이러한 최적화를 선점하기위해서 대체적인 할당문의 사용을 해서는 안됩니다.

## 딕셔너리의 할당과 복사 (Assignment and Copy Behavior for Dictionaries)
여러분이 `Dictionary` 인스턴스를 상수 또는 변수에 할당할때나 함수또는 메서드에 매개변수로 전달할때 딕셔너리는 할당이되거나 함수가 호츨되는 그 시점에 복제가 됩니다. 이 과정의 자세한 사항은 [Structures and Enumerations Are Value Types]() 항목을 참조하십시오.

만약 딕셔너리 인스턴스에 저장되어있는 키 또는 값이 값형식(구조체이거나 열거형)일 경우 그들 역시 할당될시나 함수의 호출시에 복제가 일어납니다. 이와는 다르게 만약 키 또는 값이 참조형식(클래스이거나 함수)일 경우에는 레퍼런스의 복제가 일어납니다. 하지만 이것은 그들이 참조하고 있는 클래스 인스턴스나 함수가 아닙니다. 이러한 딕셔너리의 키또는 값의 복제 방식은 구조체가 복사될때 구조체의 저장속성의 복제방식과 같습니다.

밑의 예제에서는 네 사람의 이름과 나이를 갖는 `ages`라는 딕셔너리를 정의합니다. `copiedAges`라 명명된 새로운 변수에 이 `ages` 딕셔너리를 할당합니다. 할당후에 `ages`와 `copiedAges`는 서로 다른 딕셔너리입니다.
```swift
var ages = ["Peter": 23, "Wei": 35, "Anish": 65, "Katya": 19]
var copiedAges = ages
```
이 딕셔너리의 키는 `String`타입이고 값은 `Int`타입입니다. 두 형은 Swift에서 값 타입입니다. 그러므로 딕셔너리의 복제가 일어날때 키와 값들 또한 복제됩니다.

여러분은 두 딕셔너리중에 하나의 age값을 바꾸고 확인함으로써 `ages` 딕셔너리가 복제되었음을 증명할수 있습니다. 여러분이 `copiedAges` 딕셔너리의 `"Peter"`의 값을 24로 바꿔도 `ages` 딕셔너리의 반환값은 복제가 일어나기전과 동일한 23을 반환함을 알수 있습니다.
```swift
copiedAges["Peter"] = 24
println(ages["Peter"])
// "23" 출력
```
## 배열의 할당과 복제 (Assignment and Copy Behavior for Arrays)
Swift의 배열 타입의 할당과 복제방식은 딕셔너리 타입보다 더 복잡합니다. `Array`는 여러분이 배열의 요소들을 다룰때와 복제할때 반드시 필요할 경우에만 복제를 행함으로써 C와 비슷한 성능을 제공합니다. 만약 여러분이 `Array` 인스턴스를 상수또는 변수에 할당하거나 함수 또는 메서드의 파라미터로 전달할때 배열의 요소들은 할당이 될때나 함수가 호츨될때 복제되지 않습니다. 대신 두 배열은 동일하게 정렬된 요소들의 값을 공유합니다. 여러분은 한 배열에서 요소의 값을 수정할때 다른 또 하나의 배열을 통해서 그 결과를 관찰하실 수 있습니다.

배열에서 복제는 여러분이 배열의 길이를 수정할 가능성이 있는 코드를 실행할때 일어납니다. 이것은 요소의 추가, 삽입, 삭제 또는 배열요소들의 범위를 바꾸기 위해 사용되어지는 범위지정된 subscript들을 포함합니다. 배열의 복제가 일어날때의 배열 요소들의 복제 작동방식은 [Assignment and Copy Behavior for Dictionaries]()에 설명된 딕셔너리의 키, 값의 복제와 동일합니다.

아래 예제는 `a`라 명명된 변수에 `Int`값들을 갖는 새로운 배열을 할당합니다. 그리고 이 배열은 또다시 `b`와 `c`로 명명된 두 변수에 할당됩니다.
```swift
var a = [1, 2, 3]
var b = a
var c = a
```
여러분은 supscript 문법을 통해 a 또는 b 또는 c 배열의 첫번째 값을 구할수 있습니다.
```swift
println(a[0])
// 1
println(b[0])
// 1
println(c[0])
// 1
```
만약 여러분이 supscript 문법을 통해 배열에 새로운 값을 할당하면 `a`, `b`, `c` 세개의 배열은 새로 할당된 값을 반환할것입니다. supcript 문법을 통한 단일 값의 수정은 배열의 길이를 변화시키지 않기때문에 배열의 요소에 새로운 값을 할당할때에는 복제가 일어나지 않습니다.
```swift
a[0] = 42
println(a[0])
// 42
println(b[0])
// 42
println(c[0])
// 42
```
하지만 만약 여러분이 `a`배열에 새로운 요소를 추가한다면 여러분은 배열의 길이를 수정하게 됩니다. 이것은 Swift로 하여금 요소가 추가될시에 새로운 배열의 복제를 생성하게 합니다. 더욱이 `a`는 별도의 독립적인 원배열의 복제된 배열입니다.

만약 여러분이 복제가 된후에 `a`배열의 요소를 수정하면 `a`는 여전히 원배열 요소를 참조하고 있는 `b`나 `c`와는 다른 값을 반환할것입니다.
```swift
a.append(4)
a[0] = 777
println(a[0])
// 777
println(b[0])
// 42
println(c[0])
// 42
```
## 배열 유니크 확인 (Ensuring That an Array Is Unique)
배열을 함수나 메소드에 전달하거나 배열의 요소들을 조작하기전에 그 배열이 유니크한지 확인하는것은 유용합니다. 배열형 변수의 메소드인 `unshare`를 호출함으로써 여러분은 배열의 유니크함을 확인하실 수 있습니다. (`unshare` 메소드는 상수 배열로는 호출할수 없습니다.)

만약 여러 변수들이 동일한 배열을 참조하고 있고 여러분이 그중에 하나의 변수를 이용해서 `unshare`메소드를 호출했다면 그 배열은 복제가 됨으로써 그 변수가 그 변수만의 독립적인 배열의 복사를 가지게 됩니다. 하지만 그 변수가 그 배열에 대한 유일한 참조변수라면 복제가 일어나지 않습니다.

위 예제코드의 마지막에 `b`와 `c`는 동일한 배열을 참조하고 있습니다 `b`배열 `unshare` 메소드를 호출해서 유니크한 배열을 만들도록 하겠습니다.
```swift
b.unshare()
```
만약 여러분이 `unshare` 메소드를 호출한뒤에 `b` 배열의 첫번째 요소의 값을 수정한다면 세 배열은 모두 다른 값을 보여줄겁니다.
```swift
b[0] = -105
println(a[0])
// 777
println(b[0])
// -105
println(c[0])
// 42
```
## 복수의 배열이 동일한 요소들을 공유하는지 검사 (Checking Whether Two Arrays Share the Same Elements)
식별 연산자(===와 !===)를 통해 하나 이상의 배열 또는 subarray들이 동일한 저장소와 요소들을 공유하는지를 확인할수 있습니다.

아래 예제에서는 "동일한(identical to)" 연산자(===)를 사용해서 배열 `b`와 `c`가 여전히 배열요소들을 공유하는지 확인합니다.
```swift
if b === c {
    println("b and c still share the same array elements.")
} else {
    println("b and c now refer to two independent sets of array elements.")
}
// prints "b and c now refer to two independent sets of array elements."
```
또한 식별연산자를 사용해 subarray들이 동일한 요소를 공유하는지도 검사할수 있습니다. 아래 예제는 `b`의 동일한 subarray를 비교함으로써 그 둘이 동일한 요소를 참조하고 있음을 확인합니다.
```swift
if b[0...1] === b[0...1] {
    println("These two subarrays share the same elements.")
} else {
    println("These two subarrays do not share the same elements.")
}
// prints "These two subarrays share the same elements."
```
## 강제로 배열 복제하기 (Forcing a Copy of an Array)
배열의 `copy` 메서드를 호출함으로 강제적으로 배열의 복제를 할수 있습니다. 이 메서드는 얕은복제(shallow copy)를 행하며 복사된 요소들을 갖는 새로운 배열을 반환합니다.
아래 예제에서 우리는 `names`라는 배열을 정의하고 7명의 이름을 저장합니다. `copiedNames`로 명명된 새로운 변수에 `names`배열의 `copy`메소드를 호출하고 결과값을 할당합니다.
```swift
var names = ["Mohsen", "Hilary", "Justyn", "Amy", "Rich", "Graham", "Vic"]
var copiedNames = names.copy()
```
여러분은 둘중 하나의 배열 요소의 값을 수정하고 다른 배열에서의 요소값을 확인함으로써 `names` 배열의 복제가 제대로 이루어졌는지 확인하실수 있습니다.
만약 여러분이 `copiedNames` 배열의 첫번째 요소의 값을 `Mohsen`에서 `"Mo"`로 수정해도 `names`배열은 여전히 복제가 일어나기전의 원래 값인 `"Mohsen"`을 반환합니다.
```swift
copiedNames[0] = "Mo"
println(names[0])
// "Mohsen" 출력
```
> NOTE
여러분이 단지 특정배열이 존재하는 유일한 레퍼런스임을 확실시 하시고 싶으시다면 `copy`가 아닌 `unshare`메소드를 호출하십시오. `unshare`메소드는 필요한 경우가 아닐경우 배열의 복제를 생성하지 않습니다. 반면 `copy` 메소드는 그 배열이 다른 배열과 공유하고 있지 않더라도 언제나 복제 배열을 생성합니다.
