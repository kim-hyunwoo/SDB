# 정규표현식 공부

정규식은 문자열에서 문자 조합에 일치 시키기 위해 사용되는 패턴. 자바스크립트에서, 정규식 또한 객체이다.

[regex_doc](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/%EC%A0%95%EA%B7%9C%EC%8B%9D)

# 정규식 만들기

아래와 같이 사용한다. 정규식이 지속될 때 이 방법은 유용하다.

생성자 함수를 사용하면 정규식의 런타임 컴파일을 제공한다. 정규식 패턴이 바뀌는 것을 알거나 패턴을 잘 모를때, 사용자 입력과 같이 다른 소스로부터 패턴을 얻어올 때, 생성자 함수를 쓰는게 좋다.

```javascript
var regex = /Tom/;

//또는 RegExp 객체의 생성자 함수를 부른다.
var regex = new RegExp("Tom");
```

# 문자 하나 찾기

### 문자 그대로의 표현식
문자 그대로도 정규 표현식이다. 아래 예제에서 'Tom' 그 자체도 표현식이다.
```javascript
var text = "Hello, my name is Tom. Please visit my website at http://www.Tom.com";

var regex = new RegExp("Tom");
var myArray = regex.exec(text);
console.log(myArray);

var text2 = text.replace(regex, "Howard");
console.log(text2);
//Hello, my name is Howard. Please visit my website at http://www.Tom.com
```

Tom이라는 표현식과 일치하는 단어 중 가장 앞쪽에 위치한 것만 반영된다. 전체를 다 검색, 치환을 위해선 g 플래그를 붙여줘야 한다. 나중에 살펴볼 것이다.

정규표현식에서는 대소문자를 구별하기 때문에 Tom과 tom은 일치하지 않는다. 자바스크립트에선 i 플래그를 사용해 대소문자 구별을 무시하고 검색할 수 있다.

### .은 문자 하나를 의미한다.

마침표(.)는 아무 문자 하나와 일치한다. 마침표는 어떠한 문자나 알파벳, 숫자, 심지어는 문장부호로 쓰인 마침표(.) 자체와도 일치한다. 마침표는 문자 하나를 의미하므로, 마침표가 두 개 쓰인다면 (..) 문자열 두 개를 의미한다. 만약 마침표(.) 그 자체를 검색하길 원한다면 escape 하면 된다(\.).
```javascript
var text =
`
Sales2.txt
sales1.txt
setting.txt
Sales3.txt
cool.txt
`;

var regex = /sales./gi;
//or
var regex = new RegExp("sales.", "gi");

var myArray;
while((myArray = regex.exec(text)) !== null) {
  var msg = `Found ${myArray[0]} : `;
  msg += `It starts at index ${myArray.index}`;
  console.log(msg);
}
//Found Sales2 : It starts at index 1
//Found sales1 : It starts at index 12
//Found Sales3 : It starts at index 35
```

이제 g 플래그와 i 플래그를 사용했다. 만약 i 플래그를 붙이지 않는다면, Sales와 sale를 다른 문자로 인식하고 검색 조건에 해당하는 sales만 찾을 것이다. g 플래그를 붙였기 때문에 전체 문자열을 대상으로 검색을 수행한다.

> 일치영역은 문자열 전체가 아닌 패턴과 일치하는 문자들인 경우가 많다. 위 예제에서도 전체 파일명인 sales1.txt와 일치하는게 아니고 파일명의 일부인 sales1과 일치한다. 이런 차이는 정규표현식 결과를 다른 코드나 애플리케이션에 넘겨 처리하려 할 때 중요하기 때문에 기억해야 한다.





















