# Date

### 오늘날짜 출력하기

```jsx
// document.getElementsByTagName('button'); 이 코드는 실행해도 값이 안나옴
      // 제일 위에 있기 때문 (함수는 호출했을 때 작동하니까 차이가 있다)
onload=function(){ // => 화면이 전부 로딩된 후 실행되는 코드
    document.getElementsByTagName('button')[0].onclick = testDate01; //Elements 는 nodeList 형식으로 반환
    //콜백 함수로써, testDate01뒤에 ()가 붙으면 onload될 때 바로 실행된다. 따라서 ()를 빼줘서 onclick 이벤트 발생할 때 실행되도록 한다.
    document.getElementsByTagName('button')[1].onclick = testDate02;
}
```

- onload ⇒ 화면이 전부 로딩된 후 실행되는 코드
- 콜백함수로써 뒤에 ()가 붙으면 onload될때 바로 실행되니, 이벤트 발생되면 실행되게 하기 위해서 ()를 뺀다

```jsx
function testDate01(){
    var inputDate = document.getElementById('today');
    var date = new Date(); //date라는 Date의 객체 생성

    inputDate.innerHTML += date.toDateString() + '<br>';            //Tue Jun 07 2022
    inputDate.innerHTML += date.toLocaleDateString() + '<br>';      //2022. 6. 7. LocaleDateString 은 위치 기반으로 해당 국가에서 사용하는 형식으로 반환(우리나라 식)
    inputDate.innerHTML += date.toLocaleString() + '<br>';          //2022. 6. 7. 오후 5:15:19
    inputDate.innerHTML += date.toLocaleTimeString() + '<br>';      //오후 5:15:19
}
```

- LocaleDateString() 은 위치 기반으로 해당 국가에서 사용하는 형식으로 반환(우리나라 식)
- 위 toDateString 등은 메서드기 때문에 () 붙여줌

[Date.prototype[@@toPrimitive] - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/@@toPrimitive)

date의 다양한 메서드 확인

```jsx
function testDate02(){
    var dayOfWeek = ['일', '월', '화', '수', '목', '금', '토']

    var date = new Date();
    var year = date.getFullYear(); // 2022
    var month = date.getMonth() + 1; //6 -> 1월 =  0으로 값 돌려줌
    var day = date.getDate();
    var week = date.getDay(); //=> 일~월을 0~6으로 값 반환
    document.getElementById('today').innerHTML = year + '/' + month + '/' + day + '/' + dayOfWeek[week];
}
```

2022/6/7/화

### 특정 날짜 출력

```jsx
function testDate03(){
    var year = 2022;
    var month = 9; // month는 0부터 시작하는 것을 주의
    var day = 30;
    var date = new Date(year, month - 1, day);
    document.getElementById('specific').innerHTML = date;
}
```

- new Date의 인자 안에 Date 형식으로 값을 넣어줘서 특정 날짜를 지정 가능
- month는 0부터 1월임을 주의
- 위 코드는 시간 포함이며 마지막 줄 date뒤에 .toDateString();을 붙여 날짜까지만 출력 혹은 toLocaleDateString()으로 한국식 표현 가능

### 경과날짜 구하기

```html
<h1>경과 날짜 구하기</h1>
    <label>지정 날짜</label>
    <input type="date" id="dates" size="'50">
    <br>
    <label>경과일</label>
    <input type="number" id="inputdate">
    <br>
    <label>경과 후 날짜</label>
    <input type="text" id="result" readonly>
    <br>
    <button>경과 날짜</button>
```

```jsx
function testDate04(){
    var dates = document.getElementById('dates').value;
    // console.log(dates); //input으로 입력하면 string 타입으로 저장됨(dateString)
    var date = new Date(dates);

    var inputDate = document.getElementById('inputdate').value;
    date.setDate(date.getDate() + parseInt(inputDate));
    document.getElementById('result').value = date.toLocaleDateString();
}
```

- type=’date’로 지정하면 날짜 형식으로 입력 받음
- 입력받은 값인 .value 속성을 가져와서 dates로 지정한 후 new Date로 객체 생성
- readonly 속성은 input 태그 임에도 사용자가 편집하지 못하게 함.
- getDate() 함수로 날짜를 가져오고 inputDate를 정수형으로 바꿔 연산 후 setDate 함수로 다시 날짜로 만들어 줌

### D-day 기능 만들기

```html
<h1>d-day 기능</h1>
    <label>시작 날짜</label>
    <input type="date" id="startdate">
    <br>
    <label>종료 날짜</label>
    <input type="date" id="enddate">
    <br>
    <label>남은 일수</label>
    <input type="text" id="result2" readonly>
    <button>d-day</button>
```

```jsx
function testDate05(){
    var startdates = document.getElementById('startdate').value;
    var enddates = document.getElementById('enddate').value;

    startdate = new Date(startdates);
    enddate = new Date(enddates);
    
    var result = (enddate.getTime() - startdate.getTime()) / (1000 * 60 * 60 * 24) 
    document.getElementById('result2').value = result;
}
```

- 시작날짜, 끝날짜로 입력된 value를 가지고 각각 Date의 인스턴트 생성
- getTime() 함수로 1970 년 1 월 1 일 00:00:00 UTC 기준 지난 밀리초를 계산해서 연산해줌 → 그 값은 밀리초이므로 1000으로 나눠서 초단위, 60으로 분단위, 60으로 시단위, 24로 일단위로 나눠줘서 환산 → result에는 일단위로 저장