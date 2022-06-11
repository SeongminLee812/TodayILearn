# Number

### 숫자

```jsx
function numberObj(){
      // literal
      var num01 = 3;
      // object
      var num02 = new Number(3);

      alert(num01 + '의 타입은 ' + typeof(num01));
      alert(num02 + '의 타입은 ' + typeof(num02));

      // string -> number parseInt 함수를 사용하면 정수형 자료형으로 변환
      // parseFloat 함수 -> 실수형 자료형으로 변환
      alert(parseInt('1.5') + 1);
      alert(parseFloat('1.5') + 1);

      // NaN : Not a Number
      alert(parseInt('a'));

      var num = prompt('숫자만 입력해 주세요');
      if (isNaN(num)){ //if 안 문장은 긍정인 경우 실행하게 하는 것이 관습, 앞에 !를 붙여서 t/f를 반전가능
          alert('숫자가 아닙니다.');
      } else {
          alert('숫자입니다.');
      }
  }
```

- var num01 = 3; ⇒ number 자료형 var num02 = new Number(3);  ⇒ object 자료형
- parseInt(숫자) ⇒ 정수형 자료형으로 변환 (소수점 내림)
- parseFloat(숫자) ⇒ 실수형 자료형으로 변환
- NaN ⇒ Not a Number ⇒ 숫자형 자료형이 아님(오류)

<aside> 💡 0 => 0 이라는 값 null => 변수는 있으나 값이 없음 undefined => 변수도 없는 상태 NaN => 값은 있으나 잘못된 상태(오류)

</aside>

### 난수

```jsx
function randomNum(){
      // Math 내장 모듈 사용
      // Math.random() : 0 <= x < 1 의 난수 반환
      // Math.floor() : 버림
      // Math.round() : 반올림
      // Math.ceil() : 올림

      var min = 10;
      var max = 100;
      var ran = Math.round(Math.random() * (max - min) + min)
      console.log(ran)
  }

// 배경색 랜덤으로 바꾸기
function randomBG(){
  var rnum = function(){
      return Math.round(Math.random()*256);
  }
  // style 색상 표현식 : rgb(256, 256, 256)
  document.body.style.backgroundColor = 'rgb('+rnum() + ',' + +rnum() + ','+rnum() + ')'
}
```

- Math.random() ⇒ 0≤x<1까지의 난수 반환
  - 특정 범위 내의 난수를 받고 싶은 경우 (10~100)
  - Math.floor로 감싸면 소수점 버림이 되므로 10이상 100미만

```jsx
var min = 10;
var max = 100;
var ran = Math.round(Math.random() * (max - min) + min)
```

### 랜덤 원그리기

```html
<div id="circle"></div>
<style>
    #circle{
        border: 1px solid red;
        display: none; /* div태그에 위 red로 그려지는 것을 보지 않기 위해 none */
    }
</style>
<br>
원의 넓이 : <span id="area"></span>
원의 둘레 : <span id="len"></span>
function randomCircle(){
    var rnum = Math.floor(Math.random() * 200) //반지름, 0 ~ 200 사이의 난수
    var circle = document.getElementById('circle')

    circle.style.width = rnum + 'px' //rnum은 그냥 숫자이므로 뒤에 'px 붙여줌
    circle.style.height = rnum + 'px' //circle id를 가진 div 요소에 width 값과 height 값을 rnum으로 지정

    circle.style.borderRadius = Math.floor(rnum/2) + 'px'
    // 원의 지름 = rnum / 원의 반지름만큼의 곡률을 부여함
    circle.style.display = 'block';
    return rnum; //return으로 rnum값 반환
    }

//원의 넓이, 둘레 구하는 코드(강사님)
function circleArea(){
    var circleWidth = document.getElementById('circle').style.width; // id가 circle인 요소의 style.width 요소 가져오기
// width 또는 height는 원의 지름이다 => px로 끝남

    //지름
    var r2 = parseInt(circleWidth); // 뒤에 붙은 px 빼고 정수형으로 만들어줌

    //반지름
    var r = Math.floor(r2/2);

    // 넓이
    //var area = Math.PI * r * r;
    var area = Math.PI * Math.pow(r, 2); // 제곱 구해주는 메서드

    // 둘레
    var len = Math.PI * r2;

    document.getElementById('area').innerHTML = Math.floor(area);
    document.getElementById('len').innerHTML = Math.floor(len);

    //circle이라는 변수가 circleArea 함수 내부에 없는데 왜 출력될까?
    console.log(circle);
}
// 내가 짠 코드(원의 둘레, 넓이 구하기)
var rnum = 0; // 전역변수로 선언
function randomCircle(){
    rnum = Math.floor(Math.random() * 200) //전역변수를 가져옴
    var circle = document.getElementById('circle')

    circle.style.width = rnum + 'px'
    circle.style.height = rnum + 'px'

    circle.style.borderRadius = Math.floor(rnum/2) + 'px'
    // 원의 지름 = rnum / 원의 반지름만큼의 곡률을 부여함
    circle.style.display = 'block';
    return rnum; //return으로 rnum값 반환
}

function circleArea(){  // 위에서 rnum값을 가져와야함
    var circleArea = document.getElementById('area')
    circleArea.textContent = Math.round((rnum/2)*(rnum/2)*Math.PI)
    var circleLen = document.getElementById('len')
    circleLen.textContent = Math.round(rnum*Math.PI)
}
```