# 25 고급 연산자 (Advanced Operators)
> Translator : 심상진 (dyanos@gmail.com)

기본 연산자 항목에서 설명했던 연산자들에 더하여, Swift는 훨씬 다양한 방법으로 값을 다루는 몇 개의 고급 연산자들을 제공합니다. 이들은 당신이 C와 Objective-C에서부터 친근하게 여겼던 비트를 다루는 연산자  모두를 포함합니다.

C에서의 산술 연산자들과는 다르게, Swift에서의 산술 연산자들은 기본적으로 오버플로우(Overflow)가 일어나지 않습니다. 오버플로우 동작(Overflow behavior)은 오류로써 잡히고 보고됩니다. 오버플로우 동작을 허용하기 위해서, 오버플로우를 기본으로 하는 산술 연산들 중에 Swift의 두번째 집합을 사용해야 합니다. 예를 들어, 오버플로우 덧셈(overflow addition, &+)이 그러한 집합에 속합니다. 모든  오버플로우 연산자들은 엠퍼샌드(ampersand, &)를 가지고 시작합니다.

당신이 당신 소유의 구조체들과 클래스, 그리고 열거자들을 선언할때, 이들 사용자 정의 타입들에 대해서 표준 Swift 연산자들의 독자적인 구현들(own implementations)을 제공하는데 유용할 수 있습니다. Swift는 이들 연산자들의 맞춤형(tailored) 구현들을 제공하고 그들의 행동이 당신이 만든 각각의 타입에 대해서 무엇을 해야 할지를 정확하게 결정하기 쉽게 만듭니다. 

당신은 연산자들을 재정의하는데 아무런 제한이 없습니다. Swift는 당신에게 당신 자신의 맞춤형 중위(infix), 전위(prefix), 후위(postfix) 그리고 할당 연산자들을 정의하는데 자유를 줍니다. 그리고 그것들의 우선순위와 결합순위 역시 자유롭게 정의가 가능합니다. 이들 연산자들은 마치 이미 선언된 연산자들처럼 당신의 코드 안에서 사용되고 적용될 수 있으며, 당신은 당신이 정의한 맞춤형 연산자들을 지원하도록 이미 존재하는 타입들조차 확장할 수 있습니다.

## 비트 연산자들

비트 연산자들은 당신에게 하나의 데이터 구조체 안에 있는 개개의 가공되지 않은 데이터 비트들(raw data bits)을 다루는 것을 허용합니다. 그들은 종종 그래픽 프로그래밍과 디바이스 드라이버 제작과 같은 저수준 프로그래밍에 사용됩니다. 또한 비트 연산자들은 당신이 외부의 입력들(external source)로부터 가져오는 가공되지 않은 데이터(raw data)를 가지고 작업할때 유용합니다. 예를 들어, 사용자 정의 프로토콜을 이용한 통신에서 데이터의 부호화(encoding)와 복호화(decoding)과 같은 것들이 그것입니다.

Swift는 C에서 발견되는 모든 비트 연산자들을 지원합니다. 이는 아래에서 좀더 자세히 설명드리겠습니다.

### 비트 NOT 연산자

비트 NOT 연산자(~)는 다음과 같이 숫자의 모든 비트들을 뒤집습니다.(invert)
![bitwisenot_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/bitwisenot_2x.png)

비트 NOT 연산자는 전위연산자입니다. 그리고 공백없이, 연산하는 값 바로 앞에 나타납니다.
```swift
let initialBits: UInt8 = 0b00001111
let invertedBits = ~initialBits  // equals 11110000
```
UInt8 정수들은 8개의 비트를 가지며, 0에서부터 255까지의 임의의 값을 저장할 수 있습니다. 이 예에서는 UInt8  정수 변수를, 최초의 4개 비트는 0으로, 나머지 4개비트는 1로 설정한, 이진 값 00001111을 가지도록 초기화합니다. 이것은 십진수 15와 동일한 것입니다.

다음 줄에서, 비트 NOT 연산자는 invertedBits라 불리우는 새로운 상수를 생성하는데 사용합니다. 이것은 initialBits와 동일하지만 모든 비트들이 뒤집어져 있습니다. 다시말해, 이때 initialBit의 비트들중에 0은 1이되고, 1은 0이 됩니다. "그러므로" invertedBits의 값은 11110000이 됩니다. 이것은 부호없는 십진수 240과 동일합니다.

### 비트 AND 연산자

비트 AND 연산자(&)는 두 숫자의 비트들을 결합합니다. 다음과 같이 동일 위치에 있는 비트들이 양쪽 입력 숫자들에 대해서 둘 다 1이면, 결과 값의 동일 위치에 있는 비트 역시 1로 설정되는 새로운 숫자를 돌려받습니다.("좀더 명확하게 이해되도록 수정해야 할 필요가 있음")

![bitwiseand_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/bitwiseand_2x.png)


아래의 예에서, firstSixBits변수와 lastSixBits양쪽의 값들은 4개의 중간 비트가 1로 되어있습니다. 비트 AND 연산자는 그들을 부호 없는 십진수 60과 동일한 숫자인 00111100로 만들도록 조합합니다.

```swift
let firstSixBits: UInt8 = 0b11111100
let lastSixBits: UInt8  = 0b00111111
let middleFourBits = firstSixBits & lastSixBits  // equals 00111100
```

### 비트 OR 연산자

비트 OR 연산자(|)는 두 수의 비트들을 비교합니다. 만일 다음처럼 입력 수들 중에 어떤 하나가 비트 1이면, 연산자는 해당 위치의 비트가 1로 설정된 새로운 수를 돌려줍니다.

![bitwiseor_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/bitwiseor_2x.png)

아래의 예제에서, someBits와 moreBits의 값은 서로 다른 위치에 비트 1을 가지고 있습니다. 비트 OR 연산자는 그들을 부호 없는 십진수 254와 동일한 숫자인 11111110으로 만들어지도록 조합합니다.

```swift
let someBits: UInt8 = 0b10110010
let moreBits: UInt8 = 0b01011110
let combinedbits = someBits | moreBits  // equals 11111110
```

### 비트 XOR 연산자

비트 XOR 연산자 또는 배타적(exclusive) OR 연산자 (^)는 두 수의 비트들을 비교합니다. 연산자는 다음과 같이 동일 위치에 두 입력 비트들이 서로 다른 값을 가지면 1로 같은 값을 가지면 0으로 설정된 새로운 수를 돌려받습니다.

![bitwisexor_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/bitwisexor_2x.png)

아래 예에서, firstBits와 otherBits 각각의 값들은 하나의 위치에서 1로 설정된 하지만 다른 변수에서는 그렇지 않은 비트를 가집니다. 비트 XOR 연산자는 그것들의 출력 값에서 이들 비트들의 양쪽을 1로 설정합니다. firstBits와 otherBits에서 모든 다른 비트들은 같으며, 이것은 다음과 같이 출력 값에서 0으로 나타납니다.
```swift
let firstBits: UInt8 = 0b00010100
let otherBits: UInt8 = 0b00000101
let outputBits = firstBits ^ otherBits  // equals 00010001
```

### 비트 왼쪽 및 오른쪽 쉬프트 연산자들

비트 왼쪽 이동 연산자(<<)와 비트 오른쪽 이동 연산자(>>)는 아래 정의된 규칙에 따라서, 특정 수의 위치(a certain number of places)로 모든 비트들을 왼쪽 또는 오른쪽으로 이동시킵니다.

비트 왼쪽 그리고 오른쪽 쉬프트는 2의 인수로 정수에 곱한 것과 나눈 것의 효과를 가집니다. 왼쪽으로 한 자리만큼 정수의 비트들을 이동하는 것은 값을 두 배로 하는 것과 같은 효과를 나타냅니다. 마찬가지로 오른쪽으로 이동하는 것은 2로 나누는 것과 동일한 효과를 가집니다.

#### 부호 없는 정수들에 대한 쉬프트 방법

부호 없는 정수의 비트 쉬프트는 다음처럼 합니다.

0. 존재하는 비트들은 요청된 수의 위치로(the requested number of places) 왼쪽 또는 오른쪽으로 쉬프트됩니다.
0. 정수 공간의 크기를 넘어 이동된 비트들은 버려집니다.
0. 원래의 비트들이 이동되고 남은 자리에 0이 삽입됩니다.

이 접근은 논리적 쉬프트로써 알려져 있습니다.

아래의 그림은 `11111111<<1`의 결과를 보여줍니다.(여기서는 왼쪽으로 1만큼 이동하는 것을 말합니다.) 그리고 `11111111>>1`(이것은 오른쪽으로 1만큼 이동하는 것을 말합니다.) 여기서 파란색 비트들은 쉬프트된 비트들을 말하며, 회색 비트들은 버려진 것을 말합니다. 그리고 오랜지 색의 0은 삽입된 것을 말합니다.

![bitshiftunsigned_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/bitshiftunsigned_2x.png)

여기서는 Swift 코드 안에서 어떻게 비트 쉬프트를 하는지를 다음의 실제 코드로 보여줍니다.
```swift
let shiftBits: UInt8 = 4   // 00000100 in binary
shiftBits << 1             // 00001000
shiftBits << 2             // 00010000
shiftBits << 5             // 10000000
shiftBits << 6             // 00000000
shiftBits >> 2             // 00000001
```
당신은 다음과 같이 다른 데이터 타입들 안에 있는 값들을 부호화하기 위해서 그리고 복호화하기 위해서 비트 쉬프트를 사용할 수 있습니다.
```swift
let pink: UInt32 = 0xCC6699
let redComponent = (pink & 0xFF0000) >> 16    // redComponent is 0xCC, or 204
let greenComponent = (pink & 0x00FF00) >> 8   // greenComponent is 0x66, or 102
let blueComponent = pink & 0x0000FF           // blueComponent is 0x99, or 153
```
이 예제는 핑크색에 대한 Cascading Style Sheets 색 값을 저장하기 위해 pink로 불리는 UInt32 타입의 상수를 선언합니다. CSS 컬러 값 #CC6699는 Swift의 16진수 표현으로 0xCC6699가 됩니다. 이 색깔은 비트 AND 연산자(&)와 비트 오른쪽 쉬프트 연산자(>>)를 사용하여 빨간색 (CC), 녹색(66), 파란색 (99) 요소들로 나눌 수 있습니다..

빨간색 요소는 숫자 0xCC6699와 0xFF0000사이에 비트 AND 연산을 수행함으로써 얻어집니다. 6699를 무시하기 위해서 그리고 결과에서 0xCC0000를 남기기 위해서, 0xFF0000에서의 0은 0xCC6699의 두 번째와 세 번째 바이트를 효과적으로 가려줍니다.(mask)

그때 이 수는 오른쪽으로 16칸 쉬프트(>>16)합니다. 16진수에서의 두 자리는 2진수의 8비트와 같습니다, 그래서 오른쪽으로 16칸 쉬프트은 0xCC0000를 0x0000CC로 변환할 것 입니다. 이것은 10진수 204인 0xCC와 같습니다.

비슷하게, 녹색 요소는 출력으로써 0x006600을 주는 0xCC6699와 0x00FF00사이에 비트 AND 연산을 수행함으로써 얻어집니다. 이 출력은 오른쪽으로 8칸 쉬프트되고, 10진수로 102에 해당하는 0x66의 값을 줍니다. 

마지막으로, 파란색 요소는 출력으로 0x000099를 주는 0xCC6699와 0x0000FF사이의 비트 AND 연산을 수행함으로써 얻어집니다. 여기서는 오른쪽으로의 쉬프트가 필요 없습니다. 이미 0x000099는 10진수로 153에 해당하는 0x99와 동일하기 때문입니다.

#### 부호 있는 정수에서의 쉬프트 동작(behavior)

부호 있는 정수에 대해서 쉬프트를 하는 것은 부호 없는 정수 때보다 더 복잡합니다. 이는 부호 있는 정수를 이진수로 표현하는 방식 때문입니다. (아래 예들은 간단함을 위해 8비트 부호 있는 정수들을 기본으로 하여 진행됩니다. 그러나 어떠한 크기의 부호 있는 정수에도 앞으로 나올 원칙을 적용할 수 있습니다.)

부호 있는 정수들의 (부호 비트로 알려진) 첫 번째 비트는 그 정수가 양의 정수인지 음의 정수인지를 나타내는데 사용합니다. 부호비트가 0이면 양수를, 부호비트가 1이면 음수를 의미합니다.

값 비트로 알려진 (부호 비트를 제외하고) 남은 비트들은 실제 값을 저장합니다. 양의 정수는 정확하게 부호 없는 정수에 대해서 하는 것과 같은 방법인 0부터 위쪽으로 계산하는 방법(counting upwards from 0)으로 저장합니다. 여기서는 어떻게 Int8안에서 숫자 4를 표현하는지 보여줍니다.
 
![bitshiftsignedfour_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/bitshiftsignedfour_2x.png)


부호 비트가 0(즉, 양수)이고, 7개의 값 비트들은 단지 이진 표현으로 쓰여진 숫자 4를 의미합니다.

그렇지만 음수는 다르게 저장됩니다. 2의 n승에서 그들의 절대값을 뺌으로써 저장됩니다. 이때 n은 값 비트의 수를 의미합니다. 8비트 수는 7개의 값 비트를 가집니다. 그래서 이것은 2의 7승 또는 128을 의미합니다.

여기서는 어떻게 Int8에서 -4를 표현하는지 보여줍니다.
![bitshiftsignedminusfour_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/bitshiftsignedminusfour_2x.png)

이번에는, 부호 비트가 1(즉, 음수)이고, 7개의 비트는 이진 값으로 (128 - 4인) 124를 가집니다.
![bitshiftsignedminusfourvalue_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/bitshiftsignedminusfourvalue_2x.png)

음수에 대한 부호화 방법은 2의 보수 표현법으로써 알려져 있습니다. 이것은 이상한 방법처럼 보이지만, 이러한 방법은 몇 가지 이득을 가집니다.

첫 번째, 다음과 같이 (부호 비트를 포함하는) 모든 8개의 비트들에 대해서 표준 이진 덧셈을 하고, 8비트에 적합하지 않은 어떤 것도 버릴 필요 없이 간단하게 -1을 -4에 더할 수 있습니다.
![bitshiftsignedaddition_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/bitshiftsignedaddition_2x.png)

두 번째, 2의 보수 표현은 당신에게 양수에서와 같이 음수의 비트들을 왼쪽 또는 오른쪽으로 이동시키고, 여전히 왼쪽 이동에 대해서 그들을 배가하거나 오른쪽 쉬프트 함으로써 반분되도록 합니다. 이것을 이루기 위해서, 부호 있는 정수를 오른쪽으로 이동시킬 때 다음의 추가적인 규칙들이 적용됩니다.

당신이 오른쪽으로 부호 있는 정수를 이동시킬 때, 부호 없는 정수에서와 같은 규칙들을 적용하면 됩니다만 부호와 함께 왼쪽에 있는 임의의 빈 비트들을 0과는 다른 것으로 채워야 합니다.
![bitshiftsigned_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/bitshiftsigned_2x.png)

이러한 행동은 부호 있는 정수들이 오른쪽으로 쉬프트 후에도 같은 부호를 가지는 것을 확실히 하기 위해서 입니다. 그리고 이러한 행동은 산술 쉬프트(arithmetic shift)이라고 알려져 있습니다.

양수와 음수가 저장되는 특별한 방식 때문에, 그들 중에 하나를 오른쪽으로 쉬프트하는 것은 그들의 값을 0에 더 가깝게 쉬프트 시킨다는 것을 의미합니다. 이렇게 쉬프트하는 동안 부호 비트를 동일하게 유지하는 것은 그들의 값을 0에 더 가깝게 쉬프트하는 동안에도 그 값을 음수로 남아있게 한다는 것을 의미합니다.

## 오버플로우 연산자들

만일 당신이 해당 타입의 변수가 가질 수 없는 값을 정수 상수 또는 변수에 숫자의 대입을 시도한다면, 기본적으로 Swift는 유효하지 않은 값이 생성되기를 허락하기 보다는 오류를 보고 합니다. 이 행동은 당신이 너무 크거나 너무 작은 숫자들을 가지고 작업할 때 추가적인 안전함(extra safety)을 당신에게 제공합니다.

예를 들어, Int16 정수 타입은 -32768부터 32767까지의 임의의 부호 있는 정수를 가지고 있을 수 있습니다. UInt16 상수 또는 변수에 이 범위를 벗어나는 수를 설정하려고 노력하는 것은 오류를 일으킵니다.
```swift
var potentialOverflow = Int16.max
// potentialOverflow는 3276과 동일합니다. 이것은 Int16이 가질 수 있는 가장 큰 값입니다.
potentialOverflow += 1
// 이것은 오류를 발생합니다.
```
값이 너무 크거나 너무 작을 때 에러 핸들링을 제공하는 것은 경계 값 조건과 관련된 코딩을 할 때 훨씬 더 많은 유연성을 당신에게 줍니다.

그렇지만, 당신이 사용 가능한 비트들의 수를 일부로 줄이기 위해서 오버플로우 조건을 특별히 원할 때, 당신은 오류를 일으키는 것보다 다음의 행동으로 이를 수행할 수 있습니다. Swift는 정수 계산에 대해서 오버플로우 동작을 수행할 수 있는 다섯 가지의 오버플로우 연산자들을 제공합니다. 이들 연산자들 모두는 앰퍼센트(&)를 가지고 시작합니다.

- Overflow addition (&+)
- Overflow subtraction (&-)
- Overflow multiplication (&\*)
- Overflow division (&/)
- Overflow remainder (&%)

### 값 오버플로우

여기서는 오버플로우 덧셈 연산자(&+)를 사용하여, 부호 없는 값이 오버플로우가 허용될 때 무슨 일이 일어나는지에 대한 예를 보여줍니다.
```swift
var willOverflow = UInt8.max
// willOverflow는 255와 동일합니다. 이것은 UInt8이 가질 수 있는 최대 값입니다.
willOverflow = willOverflow &+ 1
// willOverflow는 지금부터 0과 동일합니다.
```
변수 willOverflow는 UInt8이 가질 수 있는 최대 값(즉, 255 또는 이진수로 11111111)으로 초기화되어 있습니다. 그때 오버플로우 덧셈 연산자(&+)를 사용하여 1을 증가시킵니다. 이것은 그것들의 이진 표현을 UInt8의 크기를 넘도록 밀어내는데, 이것은 아래 그림에서 보여지듯이 UInt8이 가질 수 있는 값의 범위를 넘어서게 되고 오버플로우를 발생시킵니다. 오버플로우 덧셈 이후로 UInt8의 범위 안에 남아있는 값은 00000000 또는 0입니다.
![overflowaddition_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/overflowaddition_2x.png)

### 값 언더플로

숫자들은 또한 너무 작아서 그들 타입의 최대 범위에 안 맞게 될 수도 있습니다. 여기에 예제가 있습니다.

UInt8가 유지할 수 있는 가장 작은 수는 0(즉, 8비트 이진 형태에서는 00000000이 됩니다.)입니다. 만일 당신이 오버플로우 뺄셈 연산자를 사용하여 00000000으로부터 1을 뺀다면, 그 수는 이진수 11111111 또는 십진수 255으로 꺼꾸로 넘칠 것 입니다.
![overflowunsignedsubtraction_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/overflowunsignedsubtraction_2x.png)

다음은 Swift코드 에서 어떻게 보이는 지를 나타냅니다.
```swift
var willUnderflow = UInt8.min
// willUnderflow는 UInt8이 유지할 수 있는 가장 작은 값인 0이 됩니다.
willUnderflow = willUnderflow &- 1
// 현재 willUnderflow는 255와 동일합니다.
```
유사한 언더플로는 부호 있는 정수에서 발생됩니다. 부호 있는 정수들에 대한 모든 뺄셈은 직접적인 이진 뺄셈으로써 수행됩니다. 이는 뺼셈을 하고 있는 숫자의 부분으로써 포함되어 있는 부호비트도 함께이며, 비트 왼쪽 그리고 오른쪽 연산자들에서 설명한 것과 같습니다. Int8이 가질 수 있는 가장 작은 값은 -128입니다. -128은 이진수로 10000000로 나타납니다. 오버플로우 연산자를 가지고 이 이진 수로부터 1을 빼는 것은 01111111의 이진 수를 줍니다. 이것은 부호비트를 뒤집고 양수 127을 줍니다. 이는 Int8이 가질 수 있는 가장 큰 양의 수입니다.
![overflowsignedsubtraction_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/overflowsignedsubtraction_2x.png)

다음은 Swift코드에서의 표현입니다.
```swift
var signedUnderflow = Int8.min
// signedUnderflow는 -128과 같습니다. 이는 Int8이 가질 수 있는 가장 작은 값입니다.
signedUnderflow = signedUnderflow &- 1
// signedUnderflow는 지금 127과 같습니다.
```
위에 설명된 오버플로우와 언더플로의 행동의 마지막 결과는 부호 있는 그리고 부호 없는 정수 양쪽에 대해서, 항상 오버플로우가 가장 크게 유효한 정수 값으로부터 가장 작은 것으로 반복되며, 언더플로는 가장 작은 값으로부터 가장 큰 값으로 반복됩니다.

### 0으로 나누기

0으로 숫자를 나는 것(i/0) 또는 0으로 나머지를 계산하기(i%0)를 시도하는 것은 오류를 발생시킵니다.
```swift
1:let x = 1
2:let y = x / 0
```
그렇지만 이들 연산자들(&/와 &%)의 오버플로우 버전들은 당신이 만일 0으로 나누면 0의 값을 돌려줍니다.
```swift
1:let x = 1
2:let y = x &/ 0
3:// y는 0입니다.
```
## 우선순위와 결합순위

연산자 우선순위는 다른 것보다 더 높은 우선 순위를 몇몇 연산자에게 줍니다: 이들 연산자들은 첫 번째로 계산됩니다.

연산자 결합순위는 같은 우선순위의 연산자들이 어떻게 함께 그룹화되는지 또는 왼쪽으로부터 그룹화되는지, 아니면 오른쪽으로부터 그룹화되는지를 정의합니다. "그들이 그들의 오른쪽으로 그 표현(expression)과 관련 있다는 의미 또는 "그들은 그들의 오른쪽으로 그 표현과 관련 있다는 의미로써 그것을 생각해보세요.(해석이 애매함...)

복합 표현이 계산될 곳에서 계산 순서로 계산할 때 각각의 연산자의 우선순위와 결합순위를 고려하는 것은 중요합니다. 다음은 예입니다. 왜 다음에 표현이 4일까요?
```swift
2 + 3 * 4 % 5
// 이것은 4와 동일합니다.
```
엄격하게 왼쪽에서부터 오른쪽으로 얻어질 때, 당신은 이것을 다음처럼 읽기를 기대할지도 모릅니다.

0. 2 더하기 3은 5입니다.
0. 5 곱하기 4는 20입니다.
0. 20을 5로 나누었을 때의 나머지는 0입니다.

그렇지만, 실제 답은 0이 아니라 4입니다. 더 높은 우선순위의 연산자들은 낮은 우선순위를 가진 연산자보다 먼저 계산됩니다. Swift에서는, C에서와 같이, 곱셈 연산자(\*)와 나머지 연산자(%)는 덧셈 연산자(+)보다 더 높은 우선순위를 가집니다. 결과적으로, 그들은 덧셈이 고려되기 전에 양쪽 다 계산됩니다.

그렇지만, 곱셈과 나머지 연산자는 서로에 대해서 같은 우선순위를 가집니다. 정확한 계산 순위를 얻기 위해서는, 당신은 그들의 결합순위 또한 고려할 필요가 있습니다. 곱셈과 나눗셈 양쪽은 그들의 왼쪽에서부터 결합시킵니다. 그들의 오른쪽에서 시작하는 표현의 이들 부분들 주변에 내포된 괄호를 더함으로써 이것을 생각해보세요.
```swift
2 + ((3 * 4) % 5)
```
(3 * 4)는 12입니다. 그래서 이것은 다음으로 표현됩니다.
```swift
2 + (12 % 5)
```
(12 % 5)는 2입니다. 역시 이것은 다음으로 표현됩니다.
```swift
2 + 2
```
이것의 계산은 4를 답으로써 이야기합니다.

Swift에서 연산자 우선순위와 결합순위의 완벽한 목록에 대해서는 "Expressions" 항목을 보세요.

>참고
Swift의 연산자 우선순위와 결합순위 규칙은 C와 Objective-C에서 발견되는 것보다 더 간단하고 더 쉽게 예측될 수 있습니다. 하지만, 이것은 그것들이 C를 기본으로 하는 언어들에서와 완전히 같지 않다는 것을 의미합니다. 여전히 연산자들 간의 상호작용이 이미 존재하는 코드를 Swift코드로 포팅할때 당신이 의도하는 방식으로 동작하는지에 대해서 확신을 가지고 주의 깊게 적용해야 합니다.

## 연산자 함수들

클래스와 구조체는 이미 존재하는 연산자들에 대해서 그들 자신의 구현을 제공할 수 있습니다. 이것은 이미 존재하는 연산자들을 오버로딩하는 것으로 알려져 있습니다.

아래의 예는 사용자 정의 구조에 대해서 산술 덧셈 연산자(+)를 어떻게 구현할 수 있는지를 보여줍니다. 산술 덧셈 연산자는 두 개의 대상에서 동작하기 때문에 2항 연산자이며, 그것이 이들 두 개의 대상 사이에서 나타나기 때문에 중간연산자라고 불릴 수 있습니다.

예는 2차원 위치 벡터 (x, y)에 대한 Vector2D 구조체를 정의합니다. 여기서 Vector2D 구조체의 인스턴스들을 함께 더하기 위한 연산자 함수의 정의가 뒤따릅니다.
```swift
struct Vector2D {
    var x = 0.0, y = 0.0
}
@infix func + (left: Vector2D, right: Vector2D) -> Vector2D {
    return Vector2D(x: left.x + right.x, y: left.y + right.y)
}
```
연산자 함수는 '+'이라고 불리는 전역 함수로써 선언됩니다. 이 함수는 두 개의 입력 파라메터로 Vector2D 타입을 가지며, 하나의 단일 출력 값을 돌려줍니다. 이때 출력 값의 타입은 Vector2D입니다. 당신은 @infix라는 속성을 연산자 함수 선언할 때 'func' 키워드 앞에 씀으로써 중간 연산자를 구현하는 것이 됩니다.

이 구현에서, 입력 파라메터들은 '+' 연산자의 왼쪽과 오른쪽에 있는 타깃들을 Vector2D 인스턴스로 표현하는 left와 right라는 변수로 이름 지어져 있습니다. 이 함수는 새로운 Vector2D 인스턴스를 돌려줍니다. 새로운 인스턴스의 x와 y는 더해지는 두 개의 Vector2D 인스턴스들로부터 x속성들의 합과 y속성들의 합으로써 초기화 됩니다.

함수는 Vector2D 구조체상의 하나의 함수로써가 아닌, 전역적으로 정의됩니다. 그것은 존재하는 Vector2D 인스턴스들 사이의 중간 연산자로써 사용되기 위해서 입니다.
```swift
let vector = Vector2D(x: 3.0, y: 1.0)
let anotherVector = Vector2D(x: 2.0, y: 4.0)
let combinedVector = vector + anotherVector
// combinedVector는 (5.0, 5.0)의 값을 가진 Vector2D 구조체의 인스턴스입니다.
```
이 예는 아래의 그림처럼 두 벡터 (3.0, 1.0)과 (2.0, 4.0)을 벡터 (5.0, 5.0)으로 만들기 위해서 더 합니다.
![vectoraddition_2x.png](https://raw.githubusercontent.com/lean-tra/Swift-Korean/master/images/vectoraddition_2x.png)

### 전위 연산자와 후위 연산자들

위에서 보여준 예는 2항 중간 연산자의 사용자 정의 구현을 설명한 것 입니다. 클래스와 구조체들은 표준 단항 연산자들의 구현을 제공해줄 수 있습니다. 단항 연산자들은 단일 타깃에 대해서 동작합니다. 만일 그것들이 그들의 타깃보다 앞서서 나타난다면(예를 들어 -a와 같은) 전위 연산자이고, 반대로 그들의 타깃 뒤에서 나타난다면(i++과 같은) 후위 연산자라고 말합니다.

당신은 연산자 함수를 선언할 때 'func' 키워드 앞에 '@prefix' 또는 '@postfix' 속성을 사용함으로써 전위 또는 후위 단항 연산자를 구현합니다.
```swift
@prefix func - (vector: Vector2D) -> Vector2D {
    return Vector2D(x: -vector.x, y: -vector.y)
}
```
위의 예는 Vector2D 인스턴스에 대해서 단항 뺄셈 연산자(-a)를 구현합니다. 단항 뺄셈 연산자는 전위 연산자이고, 그래서 이 함수는 '@prefix'속성으로 전위연산자임을 알려주어야 합니다.

간단한 수치 값들에 대해서, 단항 뺄셈 연산자는 양수를, 부호를 뒤집을 때 같아지는 음수로 변환합니다. Vector2D에 대한 동일한 구현은 x와 y속성들 양쪽에 이 동작을 수행합니다.
```swift
let positive = Vector2D(x: 3.0, y: 4.0)
let negative = -positive
// 음수는 (-3.0, -4.0)의 값을 가지는 Vector2D 인스턴스가 됩니다.
let alsoPositive = -negative
// alsoPositive는 (3.0, 4.0)의 값을 가지는 Vector2D 인스턴스가 됩니다.
```
### 복합 할당 연산자

복합 할당 연산자들은 다른 동작에 할당(=) 연산자를 결합한 것 입니다. 예를 들어, 덧셈 할당 연산자(+=)는 하나의 동작 안에 덧셈과 할당 연산을 합친 것 입니다. 복합 할당 연산자를 구현하는 연산자 함수는 '@assignment' 속성을 기술함으로써 결합 할당 연산자임을 알려주어야 합니다. 당신은 또한 복합 할당 연산자들의 왼쪽 입력 파라메터들을 'inout'으로써 표시해야만 합니다. 이것은 파라메터의 값이 연산자 함수 안에서 직접적으로 수정될 것이기 때문입니다.

아래 예는 Vector2D 인스턴스들에 대해서 덧셈 할당 연산자 함수를 구현한 것 입니다.
```swift
@assignment func += (inout left: Vector2D, right: Vector2D) {
    left = left + right
}
```
덧셈 연산자는 더 먼저 정의되었기 때문에, 당신은 덧셈 절차를 여기서 다시 구현할 필요가 없습니다. 대신에 덧셈 할당 연산자 함수는 존재하는 덧셈 연산자 함수의 이점을 가져오고, 그것은 왼쪽 값을 오른쪽 값과 더하여 왼쪽 값에 설정하기 위해서 그것을 사용합니다.
```swift
var original = Vector2D(x: 1.0, y: 2.0)
let vectorToAdd = Vector2D(x: 3.0, y: 4.0)
original += vectorToAdd
// original은 현재 (4.0, 6.0)의 값을 가집니다.
```
당신은 '@prefix'또는 '@postfix' 속성 둘 중에 하나를 '@assignment'속성과 함께 결합할 수 있습니다. 이는 Vector2D 인스턴스에 대해서 전위 증가 연산자 (예로 ++a)의 구현에서 사용할 수 있습니다.
```swift
@prefix @assignment func ++ (inout vector: Vector2D) -> Vector2D {
    vector += Vector2D(x: 1.0, y: 1.0)
    return vector
}
```
위의 전위 증가 연산자 함수는 초기에 정의된 덧셈 할당 연산자의 이득을 취합니다. 그것은 그것이 불려진 곳 상에서 x값과 y값으로 1.0을 가지는 Vector2D를 더합니다. 그리고 결과를 돌려줍니다.
```swift
var toIncrement = Vector2D(x: 3.0, y: 4.0)
let afterIncrement = ++toIncrement
// toIncrement는 지금 (4.0, 5.0)의 값을 가집니다.
// afterIncrement는 또한 (4.0, 5.0)의 값을 가집니다.
```
>주목
기본 할당 연산자(=)를 오버로드하는 것은 불가능합니다. 단지 복합 할당 연산자들만이 오버로드됩니다. 비슷하게 3항 조건 연산자(a ? b : c)는 오버로드될 수 없습니다.

### 동등 연산자들

사용자 정의 클래스와 구조체들은 동등 연산자들, 즉 "같음(equal to)" 연산자 (==)와 "다름" 연산자(!=)로써 알려져 있는 연산자들의 기본 구현들을 받지 못 합니다. Swift에서는 당신 자신의 사용자 정의 타입에 대해서 "같음"으로 인정될 수 있는 것에 대한 추측하는 것이 불가능합니다. 이것은 "같음"의 정의가 당신의 코드에서 이들 타입들이 수행하는 역할에 의존하기 때문입니다.

사용자가 만든 타입의 동등성 검사를 위한 동등성 연산자를 사용하기 위해서는 다른 중위 연산자들에 대해서와 같이 연산자들의 구현을 제공해야 합니다.
```swift
@infix func == (left: Vector2D, right: Vector2D) -> Bool {
    return (left.x == right.x) && (left.y == right.y)
}
@infix func != (left: Vector2D, right: Vector2D) -> Bool {
    return !(left == right)
}
```
위의 예는 두 개의 Vector2D 인스턴스가 동등함 값을 가지는지에 대해서 검사하기 위해서 "같음" 연산자(==)를 구현하는 것입니다. Vector2D의 컨텍스트에서 그것은 "같음"을 "양쪽 인스턴스가 같은 x값과 y값들을 가진다"는 의미로써 고려되는 것이 이치에 맞습니다. 그래서 이것은 연산자 구현에 의해서 사용된 논리입니다. 예는 또한 "같지 않음" 연산자(!=)를 구현합니다. 이것은 간단하게 "같음" 연산자의 결과에 역을 돌려줍니다.

당신은 지금 두 개의 Vector2D 인스턴스들이 같은지 아닌지를 검사하는데 이들 연산자들을 사용할 수 있습니다.
```swift
let twoThree = Vector2D(x: 2.0, y: 3.0)
let anotherTwoThree = Vector2D(x: 2.0, y: 3.0)
if twoThree == anotherTwoThree {
    println("These two vectors are equivalent.")
}
// prints "These two vectors are equivalent."
```
## 사용자 정의 연산자들

당신은 Swift에 의해서 제공되는 표준 연산자들뿐만이 아니라 당신 소유의 사용자 정의 연산자들을 선언하고 구현할 수 있습니다. 사용자 정의 연산자들은 문자들 / = - + * % < > ! & | ^ . ~.를 가지고 단지 정의될 수 있습니다.

새로운 연산자들은 연산자 키워드를 사용하여 전역 수준에서 정의되고, 전위, 중위 또는 후위로써 정의될 수 있습니다.
```swift
operator prefix +++ {}
```
위의 예는 '+++'라고 불리는 새로운 전위 연산자를 정의합니다. 이 연산자는 Swift에서 미리 정의된 의미를 가지고 있지 않습니다. 그래서 Vector2D 인스턴스들과 함께 동작하는 특정 컨텍스트 안에서 아래와 같이 의미를 부여는 자신 소유의 사용자 정의 연산자를 선언할 수 있습니다. 이 예제의 목적을 위해서, '+++'를 새로운 "전위 두 배 증가" 연산자로써 다룹니다. 그것은 이전에 정의했던 덧셈 할당 연산자를 통해 그 자신을 그 벡터에 더하므로 써, Vector2D 인스턴스의 x와 y값을 두 배가 증가 시킵니다.
```swift
@prefix @assignment func +++ (inout vector: Vector2D) -> Vector2D {
    vector += vector
    return vector
}
```
'+++'의 이 구현은 Vector2D에 대해서 '++'의 구현과 매우 비슷합니다. 단지 이 연산자 함수가 Vector2D(1.0, 1.0)을 더하는 것 보다, 벡터를 그 자신에 더한다는 것을 제외하고는 같습니다.
```swift
var toBeDoubled = Vector2D(x: 1.0, y: 4.0)
let afterDoubling = +++toBeDoubled
// toBeDoubled는 지금 (2.0, 8.0)의 값들을 가집니다.
// afterDoubling은 또한 (2.0, 8.0)의 값들을 가집니다.
```
### 사용자 정의 중간 연산자들에 대한 우선순위와 결합순위

사용자 정의 중위 연산자들 또한 우선순위와 결합순위를 나열할 수 있습니다. 이들 두 개의 문자를 가진 연산자들이 다른 중위 연산자들과 중위 연산자들의 상호작용에 어떻게 영향을 미치는지에 대한 설명을 위해서 'Precedence and Associativity'장을 보세요.

결합순위에 대해서 가능한 조건들은 왼쪽, 오른쪽, 그리고 아무것도 아닌 쪽이 있습니다. 왼쪽 결합 연산자들은 만일 같은 우선순위를 가진 다른 왼쪽 결합 연산자들 옆에 쓰여져 있다면 왼쪽으로 결합합니다. 유사하게, 오른쪽 결합 연산자들은 같은 우선순위의 다른 오른쪽 결합 연산자들이 옆에 쓰여져 있을 경우 오른쪽으로 결합니다. 아무 쪽도 아닌 결합 연산자들은 같은 우선 순위를 가진 다른 연산자들 옆에 쓰여질 수 없습니다.

결합 방법에 대한 조건은 특별히 이야기되지 않는다면 아무 쪽도 아닌 게 기본입니다. 우선순위의 경우 특별히 이야기되지 않는다면 100이 기본입니다.

다음의 예제는 '+-'라고 불리는 새로운 사용자 정의 중위 연산자를 정의합니다. 이때 이 연산자는 왼쪽 결합이며 140의 우선순위를 가집니다.
```swift
operator infix +- { associativity left precedence 140 }
func +- (left: Vector2D, right: Vector2D) -> Vector2D {
    return Vector2D(x: left.x + right.x, y: left.y - right.y)
}
let firstVector = Vector2D(x: 1.0, y: 2.0)
let secondVector = Vector2D(x: 3.0, y: 4.0)
let plusMinusVector = firstVector +- secondVector
// plusMinusVector는 (4.0, -2.0)의 값들을 가지는 Vector2D 인스턴스입니다.
```
이 연산자는 두 벡터의 x값들을 더하고 첫 번째 것의 y로부터 두 번째 벡터의 y값을 뺍니다. 그것은 본질적으로 덧셈 연산자이기 때문에, '+'나 '-'와 같은 기본 덧셈 중위 연산자들과 같은 결합순위와 우선순위(왼쪽, 그리고 140)가 주어집니다. 기본적인 Swift 연산자 우선순위 및 결합순위 설정에 대한 완벽한 목록에 대해서는 "Expressions"장을 참조하세요.
