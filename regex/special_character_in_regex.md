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

## (?:x)

'x'에 일치하지만 일치한 것을 기억하지 않는다. 비포획괄호(non-capturing parentheses)라고 불리우고, 정규식 연산자가 같이 동작할 수 있게 하위 표현을 정의할 수 있다.

## x(?=y)

오직 'y'가 뒤따르는 'x'에만 일치한다. lookahead라고 불린다.

> /Jack(?=Sprat)/은 'Sprat'이 뒤따르는 'Jack'에만 일치한다.

> /Jack(?=Sprat|Frost)/는 'Sprat' 또는 'Frost'가 뒤따르는 'Jack'에만 일치한다.

```javascript
var text = "foobar foojar";

var reg = /foo(?=bar)/g;
console.log(text.match(reg));// [ 'foo' ] 앞쪽의 foo만 일치한다.


var reg = /foo(?=bar|jar)/g; 
console.log(text.match(reg)); // [ 'foo', 'foo' ] 모든 foo에 일치한다.
```

## x(?!y)

오직 'y'가 뒤따라오지 않는 'x'에만 일치한다. negated lookahead라고 불린다.
```javascript
var regex = /\d+(?!\.)/g;

console.log(regex.exec("3.141")); //[ '141', index: 2, input: '3.141' ]
```

## x|y

'x'또는 'y'에 일치한다.
```javascript
var regex = /south|north/gi;

console.log(("South Korea, North Korea, Korea").match(regex)); // [ 'South', 'North' ]
```

## {n}

앞 문자가 n번 나타날 경우에 일치한다. N은 양의 정수.
```javascript
var regex = /o{2}/gi;

console.log(("cold, cool, cola").match(regex)); // cool의 'oo'에 일치한다.
```


## {n, m}

n <= m 일때 앞 문자가 최소 n개 최대 m개가 나타나면 일치한다.
```javascript
var regex = /o{1,2}/gi;

console.log(("cold, cool, cola").match(regex)); // [ 'o', 'oo', 'o' ]
```

## [xyz]

문자셋(Chracter set). 괄호안의 이스케이프 시퀀스를 포함한 어떤 한 문자에 일치한다. . 이나 * 같은 특수문자는 문자셋 안에서는 특수 문자가 아니다. 따라서 이스케이프 시킬 필요가 없다. 하이픈(-)을 이용해서 문자의 범위를 지정할 수도 있다.

```javascript
var regex = /[a-c]/gi;

console.log(("a is for abc").match(regex)); //[ 'a', 'a', 'b', 'c' ]

regex = /[a-z.]+/gi;

console.log(("www.html.com").match(regex)); //[ 'www.html.com' ]

regex = /[\w.]+/gi;

console.log(("www.html.com").match(regex)); //[ 'www.html.com' ]
```

## [^xyz]

음의 문자셋(negated character set). 괄호 안에 포함되지 않은 어떤 문자든 일치한다. 하이픈을 이용해서 문자의 범위를 지정할 수 있다.

```javascript
var regex = /[^a-c]+/gi;

console.log(("a is for abc").match(regex)); //[ ' is for ' ]
```

## [\b]

백스페이스에 일치한다.

## \b

단어의 경계와 일치한다. 단어의 경계는 단어 문자가 뒤따라오지 않는 위치나, 단어 글자의 앞에서 일치한다. **단어의 경계는 일치하는 것에 포함되지 않는다** . 

```javascript
var regex = /oon\b/gi;

console.log(("moon sun").match(regex)); //[ 'oon' ]
console.log(("moonsun").match(regex)); //[ null ]

regex = /\bmoon/gi;

console.log(("moon sun").match(regex)); //[ 'moon' ]
console.log(("moonsun").match(regex)); //[ 'moon' ]
```

## \B

두개가 모두 단어이거나, 두개가 모두 비단어 일경우 일치한다. 문자열의 시작과 끝은 비단어로 여겨진다. 띄어쓰기는 단어로 여겨짐.

```javascript
var regex = /\B../g;

console.log(("moon sun").match(regex)); //[ 'oo', 'n ', 'un' ]
```

## \d

숫자 문자에 일치한다. [0-9]와 동일하다.

## \D

숫자가 아닌 문자에 일치한다. [^0-9]와 동일하다.

## \n

줄 바꿈 문자에 일치한다.

## \s

스페이스, 탭, 폼피드, 줄 바꿈 문자등을 포함한 하나의 공백 문자에 일치한다.
```javascript
var regex = /\s\w*/g;

console.log(("moon sun").match(regex)); //[ ' sun' ]
```

## \S

공백 문자가 아닌 하나의 문자에 일치한다.
```javascript
var regex = /\S\w*/g;

console.log(("moon sun").match(regex)); //[ 'moon', 'sun' ]
```

## \t

탭 문자에 일치한다.

## \w

밑줄 문자를 포함한 영숫자 문자에 일치한다. [A-za-z0-9_]와 동일하다.

## \W

비 단어 문자에 일치한다. [^A-za-z0-9_]와 동일하다.







