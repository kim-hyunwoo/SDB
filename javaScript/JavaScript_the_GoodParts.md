# 자바스크립트 핵심 가이드

## 자바스크립트 분석

자바스크립트의 강력한 언어적 특성은 다음에서 기인한다.
> 1. 자바스크립트의 함수는 어휘적 유효범위를 가진 일급 객체(first-class object)
> 2. 주류가 된 첫 번째 람다(lambda) 언어

언어 상에 제약이 없는 객체를 일급 객체라고 한다. 즉 변수에 대입되거나 인수로 넘길 수 있고, 반환값으로 사용하거나 연산 등에 사용하는데 전혀 제약이 없는 객체이다. 

람다 함수는 익명함수를 지칭하는 용어이다. 주로 고차 함수에 인자로 전달되거나 고차 함수가 돌려주는 결과값으로 쓰인다. 

이 책에서 새로운 메소드를 정의할 때 두루 쓰이는 method라는 메소드
```javascript
Function.prototype.method = function(name, func) {
    this.prototype[name] = func;
    return this;
}
```

### 원시 타입

숫자, 문자열, 불리언, null, undefined : 변형 불가능

나머지는 모두 객체 : 변형 가능한 속성들의 집합

## 객체

객체는 참조 방식으로 전달되며, 결코 복사되지 않는다.

```javascript
var stooge = {
  "first-name" : "Jerome",
  last_name : "Howard"
}

console.log(stooge["first-name"]);
console.log(stooge.last_name)

// ||연산자를 사용하여 기본값 지정 가능 
var middle = stooge.middle_name || "unknown";
console.log(middle); //unknown
```

### Prototype

모든 객체는 속성을 상속하는 프로토타입 객체에 연결돼 있다. 객체 리터럴로 생성되는 모든 객체는 자바스크립트의 표준 객체인 Object의 속성인 prototype(Obejct.prototype) 객체에 연결된다.

```javascript
//Object 객체에 create라는 메소드 추가
if(typeof Object.create !== 'function') {
  Object.create = function (o) {
    var F = function() {};
    F.prototype = o;
    return new F();
  };
}

var another_stooge = Object.create(stooge);

another_stooge['first-name'] = 'Harry';
another_stooge.last_name = 'Moses';
another_stooge.nickname = "Moe";

console.log(another_stooge.nickname); //Moe

stooge.profession = "actor";
console.log(another_stooge.profession); //actor
// 프로토타입에 새로운 속성이 추가되면 해당 프로토 타입을
// 근간으로 하는 객체들에게는 즉각적으로 이 속성이 나타난다.
```











