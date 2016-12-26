<<컴퓨터의 힘과 이성>> 에서
> 컴퓨터 프로그래머는 스스로 책임지는 세계를 창조하는 사람이다. 사실상 무한한 복잡성의 세계가 컴퓨터 프로그램이라는 형태로 만들어질 수 있다.
> - Joseph Weizenbaum

프로그램은 생각을 토대로 만들어진다. ...

프로그램을 제어할 수 있는 상태로 유지하는 것은 프로그래밍에서 가장 중요한 문제다.

(프로그램의 크기와 복잡성은 감당할 수 없을 정도로 커져서 만들 사람조차도 혼란스럽게 만들 수 있다.)

프로그래밍이라는 예술은 복잡성을 제어하는 솜씨와도 같다. 훌륭한 프로그램은 차분하며 복잡성 안에 단순함이 깃들어 있다.

탐험을 계속하길 거부하는 프로그래머라면 분명 정체 상태에서 벗어날 수 없고, 즐거움을 잊어버리며, 프로그래밍하려는 의지를 잃을 것이다.

## 변수

변수는 상자라기보다는 촉수라고 상상해야 한다. 변수는 값을 담는게 아니라 값을 쥐고 있다. 따라서 두 변수가 같은 값을 가리킬 수 있다.

프로그램에서는 여전히 쥐고 있는 값에만 접근할 수 있다. 뭔가 기억할 필요가 있다면 촉수를 뻗어 그것을 쥐고 있거나 기존 촉수를 새 값에 붙이면 된다.

```javascript
var theNumber = Number(prompt("숫자를 입력하세요"));
alert("입력한 숫자의 제곱은 " + (theNumber * theNumber) + "입니다.");

// Number() : 값을 숫자로 변환
// String(), Boolean() 함수도 각각 해당 타입으로 값을 변환한다.
```

Number() 함수처럼 어떤 경우에는 변수의 첫 글자도 대문자로 쓸 때가 있다. 이것은 이 함수를 생성자로 표시하기 위해서이다.

## &&과 ||

||이 실제로 하는일은 다음과 같다.
- 먼저 왼쪽의 값을 살핌
- 왼쪽 값을 불리언 값으로 변환해서 true면 왼쪽 값을 반환
- 그렇지 않으면 오른쪽 값을 반환

```javascript
var input = prompt("성함이 어떻게 되나요?");
print("안녕하세요 " + (input || "고객님"));
```

true라면 input값만 반환되고, false라면 고객님을 반환한다. 사용자가 이름을 입력하지 않고 어떻게든 취소를 클릭하거나 prompt 대화상자를 닫으면 input  변수에 null 이나 "" 값이 담길 것이다. 이런 두 값은 불리언으로 변환했을 때 모두 false를 반환한다.

따라서 이 경우 input || "고객님"이라는 표현식은 input 변수의 값 혹은 "고객님"이라는 문자열로 읽힌다. 이는 '실패처리' 값을 제공하는 아주 쉬운 방법이다.

&& 연산자도 이와 비슷하게 동작하지만, 결과는 반대다. 왼쪽 값을 불리언으로 변환했을 때 false가 반환된다면 그 값을 반환하고, 그렇지 않으면 오른쪽 값을 반환한다.

이 두 연산자의 또 한 가지 특징은 오른쪽 표현식을 필요할 때만 평가한다는 점이다.
true || X의 경우 X와는 상관없이 결과는 항상 true이므로 X는 절대로 평가되지 않으며,
X에 부수효과가 있더라도 절대로 일어나지 않는다. 이것은 false && X에서도 마찬가지다.

## 함수

```javascript
function square(x) {
    return x * x;
}

console.log(square(12));
```

함수의 기본 형태는 위와 같다. function 키워드를 통해 함수를 만들고 키워드 다음에 변수 이름이 온다.
새 함수가 해당 변수 이름으로 저장된다.

컴퓨터가 모든 함수의 정의를 찾아 프로그램의 나머지 부분을 실행하기 전에 관련 함수를 저장해둔다.
때문에 함수를 정의하고 사용하는 순서에 신경 쓰지 않아도 된다. 즉, 어느 함수를 먼저 선언하느냐와 관계없이 함수를 서로 호출할 수 있다.

### 지역변수

_함수 안에서 만든 변수는 함수 안에서만 통용(지역성)_. 변수의 지역성은 함수의 인자와 함수 안에서 var 키워드로 선언한 변수에만 적용된다.

**자바 스크립트에서는 코드 블록에서 새로운 지역환경을 만들어내지 않는다.(다른 언어와의 차이)**

```javascript
var something = 1;
{
     var something = 2;
}

console.log(something); //2
// 자바스크립트에서 새로운 유효범위를 만들어내는 것은 함수밖에 없다.
```

### 스택

함수가 호출되고 반환되는 방식을 이해하려면 스택을 이해해야 한다. 함수가 호출되면 해당 함수의 본문에 제어권이 부여된다. 함수의 본문이 실행되는 동안 _컴퓨터는 나중에 실행을 계속할 지점을 알 수 있게 함수를 호출한 문맥(Context)을 반드시 기억해둬야만 하고, 이런 문맥이 저장되는 곳이 스택_이다.

### 함수값

자바스크립트에서는 함수를 비롯해 모든 것이 값이다. 이는 정의한 함수의 이름을 일반 변수처럼 쓰고 함수의 내용을 여기저기 전달하거나 더 큰 표현식에서 쓸 수 있다는 의미이다.

```javascript
var a = null;
function b() {
    return "B";
}
console.log((a || b)());
```

이름 없는 함수 값이 필요하다면 function 키워드를 표현식으로 사용하면 된다.
```javascript
var a = null;
console.log((a || function() {return "B";})());

(function() {console.log("A";)})();
// 익명 함수 표현식인 function() {return "B"; }는 함수 값을 생성하기만 한다.
```

찾아볼 부분 : 함수의 일급 특성

## 클로져

함수를 값으로 다룰 수 있다는 사실과 함수 스택의 특성을 결합하면 흥미로운 문제를 제기할 수 있다.
지역 변수를 생성한 함수의 호출이 끝나 더는 스택에 남아 있지 않으면 그런 지역 변수는 어떻게 될까?
다음 코드는 그와 같은 상황을 보여준다.

```javascript
function createFunction() {
     var local = 100;
     return function() {return local;};
}
```

createFunction()이 호출되면 지역 변수를 생성한 후 이 지역변수를 반환하는 함수를 반환한다.
이런 상황을 처리하는 방법에 관한 문제를 _상향 펀아그 문제(upward funarg problem)_라 하며,
이전의 여러 프로그래밍 언어에서는 이렇게 하지 못하게 했다.

자바스크립트는 지역 변수에 접근할 수만 있다면 지역 변수를 보존하기 위해 부단히 노력하는 식으로
이런 문제를 해결하는 언어 세대에 속한다.

이런 특징을 클로져(Closure)라고 하며, 일부 지역 변수에 ‘닫혀 있는’ 함수를 클로져라고 한다.
이런 동작 방식은 여전히 ‘살아있는’ 변수에 관한 걱정을 덜 수 있게 해줄 뿐만 아니라
함수 값을 창의적인 방식으로 사용하는 데 활용할 수 있다.

예) 다음 함수는 특정 숫자를 인자에 더하는 함수 값을 동적으로 생성할 수 있게 만들어 준다.
```javascript
function makeAdder(amount) {
     return function(number) {
          return number + amount;
     };
}

var addTwo = makeAdder(2);
console.log(addTwo(3));
```

### 선택인자

alert(“A”, “B”, “C”, “D”);
공식적으로 alert 함수는 인자를 하나만 취하지만, 위 코드 처럼 호출해도 문제가 발생하지 않는다.
다른 인자를 무시하고 A를 출력한다.

자바스크립트는 인자의 양에 관해 엄격하지 않다. 인자를 많이 전달하면 나머지 인자를 무시한다.
인자를 너무 적게 전달하면 누락된 인자의 값은 undefined가 된다.

이 방식은 실수를 유발할 수도 있지만, 다음과 같이 이용할 수도 있다.

```javascript
function power(base, exponent) {
     var result = 1;
     if(exponent === undefined) {
          exponent = 2;
     }
     for (var count = 0; count < exponent; count++){
          result *= base;
     }
     return result;
}

console.log(power(10));
```
exponent에 인자 값이 전달되지 않을 경우 square처럼 동작한다.

## 함수를 사용하기 위한 기법

1. 중복 방지

함수의 발명 이유는 코드의 일부를 재사용하기 위해서다. **가장 좋은 함수는 단 하나의 단순한 동작을 수행하는 함수** 이다. 그렇게 하면 함수의 이름을 짓기도 쉽고(이해하기도 쉽다) 더욱 다양한 상황에서 사용할 수 있기 때문이다.

한 가지 좋은 원칙은 확실히 필요하다고 생각하기 전까지는 기발한 기능을 추가하지 않는 것이다.

2. 순수함

함수에 적용된 순수함은 함수에 부수효과가 있느냐와 관련되어 있다. 순수 함수는 동일한 인자를 전달받을 경우 항상 같은 값을 반환하며, 부수효과가 없다.

3. 재귀

자기 자신을 호출하는 함수를 재귀적이라고 한다.
```javascript
function power2(base, exponent) {
     if(exponent == 0) {
          return 1;
     } else {
          return base * power2(base, exponent - 1);
     }
}
```

프로그램이 증명할 수 있을 정도로 너무 느려지기 전까지는 효율성에 관해 걱정하지 말라.
실제로 프로그램이 그런 지경에 다다르면 어느 부분에서 가장 시간이 많이 걸리는지 파악한 후
그런 부분에서 우아함과 효율성을 맞바꾸면 된다.

Q. 숫자 1에서 시작해 반복적으로 5를 더하거나 3을 곱하는 식으로 무한한 양의 새로운 숫자를 만들어 낼 수 있다. 어떤 숫자가 있을 때 그 숫자를 만들어내는 더하기와 곱하기의 나열을 찾아내는 함수를 어떻게 작성?
```javascript
function findSequence(goal) {
     function find(start, history) {
          if(start == goal) {
               return history;
          } else if (start > goal) {
               return null;
          } else {
               return find(start + 5, "(" + history + " + 5)") || find(start * 3, "(" + history + " * 3)" );
          }
     }
     return find(1, "1");
}
```

## 자료구조 : 객체와 배열

프로퍼티에는 대괄호를 사용하거나 점 표기법을 이용해 두 가지 방식으로 접근 가능하다.

```javascript
var text = “purple haze”;

text[“length”];
text.length;

// 두번 째 방법은 첫 번째 방법의 축약형. 프로퍼티의 이름이 유효한 병수명일 경우에만 작동.
// 유효한 변수명 : 변수 명에 공백이나 기호가 없으며 숫자로 시작하지 않는 경우
```

## 객체 값

대부분의 값 타입의 프로퍼티는 정해진 것이며 변경할 수 없다. 예를 들어 문자열의 길이는 항상 동일하다. 하지만 프로퍼티를 마음껏 추가하고 제거할 수 있는 객체object라는 값 타입이 있다.
_사실 객체의 주된 역할은 프로퍼티의 집합으로 사용하는 것이다_

```javascript
var cat = {
     color: "gray",
     name: "tom",
     size: 46
};

console.log(cat.size);
delete cat.size;
console.log(cat.size); // undefined
// delete라는 키워드는 프로퍼티를 삭제하는 데 사용한다.
cat.weight = 10; 
// =연산자로 프로퍼티를 객체에 추가할 수 있다.

var thing = {"gabba gabba": "hey", 5: 10};
console.log(thing["5"]);
console.log(thing[2+3]);
delete thing["gabba gabba"];
console.log(thing);
// 이름이 유효하지 않은 변수명인 프로퍼티에는 점 표기법으로 접근할 수 없으며, 대괄호로만 접근 가능하다.

// 대괄호 사이에는 어떤 표현식도 올 수 있다. 표현식은 문자열로 변환돼 해당 표현식이 가리키는 프로퍼티 이름을 결정한다. 따라서 변수를 사용해 프로퍼티의 이름을 지정할 수도 있다.

var propertyName = "length";
var text = "coco";
console.log(text[propertyName]); //4

// in 연산자를 사용하면 객체에 특정 프로퍼티가 있는지 검사할 수 있으며, 연산의 결과로 불리언 값이 반환된다.
var chineseBox = {};
chineseBox.content = chineseBox;
console.log("content" in chineseBox); //true
console.log("content" in chineseBox.content); //true
```

## 배열

객체를 사용하는 용도 중 하나가 바로 컬렉션Collection이다.

```javascript
var mailArchive = {
     0: "조카에게, ... (메일번호1)",
     1: "(메일번호2)",
     2: "(메일번호3)"
};

for(var current = 0; current in mailArchive; current++) {
     console.log("처리중인 이메일 # : " + current + "\n메일 내용 :" + mailArchive[current]);
}
```

이런 종류의 특화된 용도에 맞는 객체가 바로 배열Array이다.

```javascript
var mailArchive = ["메일1", "메일2", "메일3"];

for(var current = 0; current < mailArchive.length; current++) {
     console.log("처리중인 이메일 # : " + current + "\n메일 내용 :" + mailArchive[current]);
}
```

## 메서드

문자열과 배열 객체는 모두 length 프로퍼티와 더불어 함수 값을 가리키는 프로퍼티의 수를 담고 있다.

모든 문자열에는 toUpperCase 프로퍼티가 있다. toUpperCase는 문자열 객체의 메서드이다. 함수를 담고 있는 프로퍼티를 메서드라고 한다.

```javascript
var doh = "Doh";
typeof doh.toUpperCase; //function
doh.toUpperCase; //"DOH"

var saying = [];
saying.push("마른");
saying.push("하늘에");
saying.push("날벼락");

console.log(saying.join(" ")); //마른 하늘에 날벼락
```

```javascript
var words = "소 잃고 외양간 고친다.";
var wordsArray = words.split(" ");
console.log(wordsArray); //[ '소', '잃고', '외양간', '고친다.' ]

//split 메서드는 문자열을 배열로 쪼개며,
//인자로 전달된 문자열을 이용해 어느 부분을 기준으로 잘라야 할지 판단한다.
```


## for in

```javascript
livingCats = ["금강", "헬로"];

for (var cat in livingCats)
    console.log(cat);

// 객체에 들어있는 프로퍼티 이름을 모두 살피는데, 이렇게 하면 집합에 들어있는 이름을 모두 열거할 수 있다.
// 인덱스가 반환된다.
```

### Date

```javascript
var when = new Date(1980, 1, 1);

//현재 시간
new Date();

//2016년 12월 26일
new Date(2016, 12, 26);

//2016년 12월 26일 8시 20분 30초
new Date(2016, 12, 26, 8, 20, 30);
```

new는 객체 값을 생성하는 수단이다.

```javascript
var now = new Date();

console.log(now.getFullYear() + "년 " + (now.getMonth() + 1) + "월 " + now.getDate() + "일");

//getMonth 0 ~ 11 (0 is Jan)
//getDay 0 ~ 6 (0 is sunday)
//getDate는 1~31 (일반적인 방식)
```

### 오류 처리

```javascript
function lastElement(array) {
    if (array.length > 0)
        return array(array.length - 1);
    else
        throw "빈 배열의 마지막 요소는 취할 수 없습니다.";
}

function lastElementPlusTen(array) {
    return lastElement(array) + 10;
}

try {
    console.log(lastElementPlusTen([]));
} catch (error) {
    console.log("뭔가 잘못 됨 : ", error);
}
```

## 예외가 발생한 후의 정리

try문 다음에는 finally 키워드가 올 수 있는데, 이는 "어떤 일이 일어나든 try 블록에 있는 코드를 실행한 후 이 코드를 실행한다."라는 의미이다. 함수에서 뭔가를 정리해야 한다면 정리 코드는 대개 finally 블록에 둬야 한다.

```javascript
function processThing(thing) {
    var prevThing = currentThing;
    currentThing = thing;
    try {
        // 복잡한 처리를 수행
    } catch (e) {
        // 에러 처리
    } finally {
        currentThing = prevThing;
    }
}
```

이제 복잡한 처리과정에서 정상적으로 반환하거나 예외를 던져도 currentThing은 항상 이전 값으로 설정된다.

### 오류 객체

프로그램에 존재하는 대다수의 오류는 자바스크립트 환경에서 예외가 발생하게 만든다. 예를 들어 다음과 같은 프로그램에서는 '예외 발생: Sasquatch is not defined' 같은 내용이 출력될 것이다.
```javascript
try {
    print(Sasquatch);
} catch(e) {
    print("예외 발생: " + e.message);
}
```
이런 문제에 대해서는 특별한 타입의 객체가 발생한다. 이 객체에는 항상 문제에 대한 설명이 담긴 message 프로퍼티가 포함돼 있다. new 키워드와 Error 생성자를 이용하면 비슷한 객체가 발생하게 할 수 있으며, 이 때 메시지를 인자로 전달한다.
```javascript
throw new Error("늑대가 나타났다.");
```

테스트 작성과 실행을 용이하게 하는 다양한 프레임워크가 존재한다. 알아볼 것.

### 함수형 프로그래밍

## 추상화

함수형 프로그래밍은 함수를 영리하게 조합하는 방법을 통해 추상화를 달성한다. 기본적인 함수, 그리고 좀 더 중요한 함수의 사용법에 대한 지식으로 무장한 프로그래머는 모든 프로그램을 처음부터 새로 작성하기 시작하는 프로그래머에 비해 훨씬 더 업무를 효과적으로 처리한다. 

## 고차 함수

```javascript
//특정한 행위를 함수 값으로 전달
function forEach(array, action) {
    for (var i = 0; i < array.length; i++) {
        action(array[i]);
    }
}

forEach(["Wampeter", "Foma", "Granfalloon"], console.log);
```

익명 함수를 활용해 for 반복문과 같은 것을 좀 더 유용한 세부 사항을 채워 작성할 수 있다.

```javascript
function sum(numbers) {
    var total = 0;
    forEach(numbers, function(number) {
        total += number;
    });
    return total;
}

console.log(sum([1, 2, 3]));
```

다음과 같이 작성할 수 있다.
```javascript
var paragraphs = mailArchive[mail].split("\n");
for (var i = 0; i < paragraph.length; i++) {
    handleParagraph(paragraphs[i]);
}

//함수를 정의해서 위 코드를 다음과 같이 작성 
forEach(mailArchive[mail].split("\n"), handleParagraph);
```

대체로 추상적인(고차원적인) 구성물을 사용할수록 더 정보가 많고 잡음이 적어진다.

다른 함수를 대상으로 동작하는 함수를 고차 함수라 한다. 고차 함수는 함수를 조작하는 방식으로 새로운 차원에서의 활동에 관해 이야기 할 수 있다. 

고차함수는 평범한 함수로는 손쉽게 기술할 수 없는 각종 알고리즘을 일반화하는 데 사용할 수 있다. 현장에서는 이것이 더 짧고, 명료하고 쾌적한 코드를 의미한다.

### 함수 수정

유용한 한 가지 유형의 고차함수는 전달된 함수 값을 수정하는 함수다.
```javascript
function negate(func) {
    return function(x) {
        return !func(x);
    };
}
var isNotNaN = negate(isNaN);
console.log(isNotNaN(NaN)); //false
```

negate에서 반환한 함수에서는 자신이 받은 인자를 원본 함수인 func에 전달한 다음 결과를 부정한다.

부정하고 싶은 함수에서 인자를 한 개 이상 받는다면, apply 메서드를 사용할 수 있다. 이 메서드는 인자를 두 개 받는다.
```javascript
function negate(func) {
    return function() {
        return !func.apply(null, arguments);
    };
}
```
[doc](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

## 객체

### 메서드 정의

객체에 메서드를 부여하는 한 가지 방법은 함수 값을 객체에 첨가하는 것.
```javascript
var rabbit = {};
rabbit.speak = function(line) {
    console.log("토끼가 말한다 : " + line);
};

rabbit.speak("저는 살아있어요!");
```

this는 함수가 메서드로 호출될 때 관련 객체를 가리킨다.
```javascript
 function speak(line) {
    console.log(this.adjective + "토끼가 말한다 : " + line);
}

var whiteRabbit = {adjective: "흰색", speak: speak};
var fatRabbit = {adjective: "살찐", speak: speak};

whiteRabbit.speak("제 귀와 눈썹이 얼마나 늦게 자라는지 보세요.");
fatRabbit.speak("저는 살쪘어요.");
```

### apply()와 call()의 적용
```javascript
function speak(line) {
    console.log(this.adjective + "토끼가 말한다 : " + line);
}

var whiteRabbit = {adjective: "흰색", speak: speak};
var fatRabbit = {adjective: "살찐", speak: speak};

whiteRabbit.speak("제 귀와 눈썹이 얼마나 늦게 자라는지 보세요.");
fatRabbit.speak("저는 살쪘어요.");

speak.apply(fatRabbit, ["냠냠."]);
speak.call(fatRabbit, "꺼억~");

function run(from, to) {
    console.log(this.adjective + "토끼가 " + from +"에서 " + to + "로 달려간다.");
}

run.apply(fatRabbit, ["A", "B"]);
run.call(whiteRabbit, "A", "B");
```

### 생성자

new 키워드는 새 객체를 생성하는 편리한 수단을 제공한다. 앞에 new 연산자를 붙여 함수를 호출하면 해당 함수의 this 변수는 새로운 객체를 가리킬 것이며, 자동으로 새 객체가 반환될 것이다.

토끼의 생성자
```javascript
function Rabbit(adjective) {
    this.adjective = adjective;
    this.speak = function(line) {
        console.log(this.adjective + "토끼가 말한다." + line)
    };
 }

 var killerRabbit = new Rabbit("킬러");
 killerRabbit.speak("하하하하");
```

생성자의 이름은 대문자로 시작하는 것이 관례.

### 프로토타입 재정의 기법

토끼 객체는 Rabbit 생성자와 연관된 프로토타입을 기반으로 한다. 생성자의 prototype 프로퍼티를 이용하면 이 프로토타입에 접근할 수 있다.

정의한 모든 함수는 자동으로 prototype 프로퍼티를 갖게 되는데, 이 프로퍼티는 객체(함수의 프로토타입)를 보관한다. 이 프로토타입은 constructor 프로퍼티를 갖게 되며, 이 프로퍼티는 해당 함수가 속한 함수를 역으로 가리킨다.

토끼 프로토타입 자체는 객체이므로 Object 프로토타입을 기반으로 하며, Object 프로토타입의 toString 메서드를 공유한다. 따라서 Rabbit 생성자로 생성한 모든 토끼는 이 메서드를 갖게 된다.

객체가 그것의 프로토타입의 프로퍼티를 공유하는 것처럼 보여도 이런 공유는 단방향이다. 프로토타입의 프로퍼티는 그것을 기반으로 하는 객체에 영향을 주며, 이런 객체를 변경하더라도 프로토타입에는 아무런 영향을 미치지 않는다.

정확한 규칙 :

프로퍼티의 값을 조회할 때 자바스크립트는 먼저 객체 자체가 갖고 있는 프로퍼티를 살핀다. 찾고자 하는 이름의 프로퍼티가 있으면 그 값을 취한다. 아무런 프로퍼티도 없으면 계속해서 해당 객체의 프로토타입, 그리고 그 프로토타입의 프로토타입을 검색하는 식으로 진행한다. 아무런 값이 없으면 undefined가 반환된다.

이것은 직접 작성한 객체의 프로퍼티를 재정의해서 프로토타입에서 취한 일반화된 값으로 좀 더 구체적이고 적합한 값을 프로퍼티에 부여할 수 있다는 의미이다.(Override)

```javascript
Rabbit.prototype.teeth = "작다";
killerRabbit.teeth; //작다

Rabbit.prototype.dance = function () {
    console.log(this.adjective + "토끼가 춤춘다.");
}
```

토끼의 원형에 해당하는 토끼가 바로 speak 메서드 처럼 모든 토끼가 공통적으로 가져야 할 값을 놓을 최적의 장소이다.
```javascript
function Rabbit(adjective) {
    this.adjective = adjective;
}

Rabbit.prototype.speak = function(line) {
    console.log(this.adjective + "토끼가 말한다. " +line );
};
```











































