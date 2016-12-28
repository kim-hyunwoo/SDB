# 정규식에서의 특수문자

## 백슬래시(\\)

문자 앞의 백슬래시는 문자 그대로 해석하는 대신 특수 기능을 제공한다.

> \\b는 b와 일치하지 않으며, 단어 경계 문자의 기능을 제공한다.

특수 문자 앞에 백슬래시는 특수문자를 그대로 해석하게 한다.

> /a./는 a와 문자 하나를 의미하지만, /a\\./은 a와 . 마침표 하나를 의미한다.

## ^

입력의 시작에 일치한다. 만약 multi-line 플래그가 true로 설정되어 있다면, 줄 바꿈 문자 바로 다음에서도 일치한다.

> /^A/는 "an A"의 'A'에 일치하지 않는다. 그러나 "An E"의 'A'에서는 일치한다.

^는 문자셋[abc] 패턴의 첫 글자로 쓰이면, 다른 의미를 가진다.

## $

입력의 끝과 일치한다. 만약 multi-line 플래그가 true로 설정되어 있다면, 줄 바꿈 문자 바로 다음에서도 일치한다.

> /t$/는 "eater"의 't'에는 일치하지 않는다. 그러나 "eat"에서는 일치한다.

## \*

0회 이상 연속으로 반복되는 앞선 문자에 일치한다. {0,}과 동일하다.

> /co*/ 는 "cooooool"의 coooooo에 일치하며, "caaaar"의 'c'에 일치한다.
> 0 이상이기 때문에 0포함 0초과

## +

1회 이상 연속으로 반복되는 앞선 문자에 일치한다. {1,}과 동일하다.

> /c+/는 "cool"의 'c'에 일치하고, "ccc"의 모든 'c'에 일치한다.

## ?

0 또는 1회 반복되는 앞선 문자에 일치한다. {0, 1}과 동일하다.

> /e?le?/는 "angel"의 'el'에 "oslo"의 'l'에 일치한다.

수량자 바로 뒤에 사용한다면 수량자를 탐욕스럽지 않게 만든다.
```javascript
var text = "123abc";

var reg = /\d+?/;
console.log(text.match(reg)); // [ '1', index: 0, input: '123abc' ]

var reg = /\d+/;
console.log(text.match(reg)); // ['123', index: 0, input: '123abc' ]
```

## .

개행 문자를 제외한 어떤 하나의 문자에 일치한다.
```javascript
var text = "nay, an apple is on the tree";

var reg = /.n/g;

console.log(text.match(reg)); //[ 'an', 'on' ]
//'nay'에는 일치하지 않고, 'an'과 'on'에 일치한다.
```

## (x)

'x'에 일치하며 일치한 것을 기억한다. 포획괄호(capturing parentheses)로 불린다.

(foo) (bar)는 각 문자와 일치하는 문자를 찾아서 포획한다. \\1은 포획한 문자열 중 첫 번째에 보관된 문자열. 즉, 'foo'를 의미하고 \\2는 'bar'를 의미한다.

```javascript
var text = "foo bar foo bar";

var reg = /(foo) (bar) \1 \2/g;

console.log(text.match(reg)); // ['foo bar foo bar' ]
```

포획된 문자는 $1, $2, ... , $n 과 같은 표현으로 사용할 수 있다. 아래 예제에서 포획된 첫 번째 문자열 foo와 두 번째 문자열 bar는 각각 $1과 $2에 할당되어 있고, replace 메소드를 이용해서 대체할 수 있다.

```javascript
var text = "foo bar foo bar";

var reg = /(foo) (bar) \1 \2/g;

console.log("foo bar foo bar".replace(reg, '$2 $1 $2 $1'));
// bar foo bar foo
```






