웹은 3차원

normal flow - float - position

float시 부모에 차지하는 영역이 없어진다.

부모요소에 overflow: hidden;으로 해결

형제 요소에 clear: both;로 해결

clearfix 사용

.container::after {
  content: '';
  display: block;
  clear: both;
}

position은 좌표 설정으로 위치를 설정

기준이 되는 부모 요소에 postion: relative 속성을 선언

부모의 위치를 기준으로 움직이고자하는 자식 요소에 position:absolute를 선언하고 원하는 위치로 움직이기 위해 top, right, bottom, left 설정

html5rocks.com


가운데 정렬하기

가로로 정렬하기

margin: 0 auto; 반드시 width 값이 있어야 한다.
blocK 요소의 inline 속성 요소를 가운데 정렬할 때 text-align: center;

세로로 정렬하기

line-height 값을 부모의 높이와 동일하게 준다.

position: absolute; top: 50%; margin-top:(요소 높이의 절반값);

.box1 {
  position: absolute;
  top: 50%;
  left: 50%;
  margin-top: -50px;
  margin-left: -50px;
}

요소의 가로, 세로 값을 알 수 없을 때!
1. 부모와 자식에 position 속성, 자식요소를 가운데 정렬하길 원하는 방향으로 50% 이동
2. 그리고 transform: translate()를 통해 자동 계산된 요소의 길이만큼 -50% 이동

.box1 {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

















.
