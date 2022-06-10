# String

### 문자열 합치기

```jsx
function strTest01(){
    var str01 = 'string';
    var str02 = 'Test';

    // + 연산자
    alert(str01 + str02);

    // concat()
    var newString = str01.concat(str02, 'Java', 'Script');
    alert(newString);

    // join
    var joinTest = ['a', 'b', 'c', 'd'].join('_');
    alert(joinTest);
}
```

- 3가지 방법 사용가능
    - + 연산자 사용
    - concat 사용(`객체.concat(더할 인자)`)
    - join 함수 사용(파이썬과 동일)

### 다른 자료형 합치기

```jsx
function strTest02(){
    var a = 1;
    var b = 2.3;
    var c = false;
    var result = a + b + c + 'abc' + a + b + c; //3.3abc12.3false
    alert(result);
}
```

- 문자열과 다른타입이 만나면 전부 문자열이 된다.
- false 는 0으로 계산되고 true는 1로 계산된다.
- 파이썬과는 다르게 타입 에러가 나오지 않는다

### 문자열 비교하기

```jsx
function strTest03(){
    var str = prompt('이름을 입력해 주세요');
    var span = document.getElementById('res');

    if (str=='멀캠') {
        span.textContent = str + '님, 환영합니다.'
    } else if (str.toLowerCase() == 'multicampus'){
        span.textContent = 'hello, ' + str;
    } else {
        span.textContent = '다시 확인해 주세요';
    }

    // 동등연산자 == : 자동 형 변환 (자동으로 자료형을 같은 자료형으로 변환하여 비교한다)
    var numVal = 10;
    if (numVal == '10') {
        alert('== 연산자 사용 : 숫자 10 == 문자 10'); // 숫자 10 == 문자 10
    } else { 
        alert('== 연산자 사용 : 숫자 10 != 문자 10');
    }

    // ===연산자를 사용하면 엄격한 비교 (자료형까지 같은지 비교)
    if (numVal === '10') {
        alert('=== 연산자 사용 : 숫자 10 = 문자 10')
    } else {
        alert('=== 연산자 사용 : 숫자 10!= 문자 10')
    }

    // string object(객체)
    var strObj = new String('멀캠'); //String은 내장된 클래스
    // string literal
    var strVal = '멀캠';
    // 두개의 자료형이 다르다.

    if (strObj == strVal) {
        alert('strObj == strVal')
    } else{
        alert('strObj != strVal')
    }

    if (strObj === strVal) {
        alert('strObj === strVal')
        alert(typeof(strObj)) // object
        alert(typeof(strVal)) // string 
    } else{
        alert('strObj !== strVal')
        alert(typeof(strObj))
        alert(typeof(strVal))
    }
}
```

- 동등연산자 == : 자동 형변환
자동으로 자료형을 같은 자료형으로 변환하여 비교한다
숫자 10 == 문자 10
- 엄격한 비교 연산자 === : 자료형 까지 같은 지 비교한다.
숫자 10 ≠ 문자 10
- object(객체)
`var strObj = new String('멀캠');`
 string(문자열)
`var strVal = '멀캠';` ⇒ 두개의 자료형이 다르다
- 동등 연산자를 사용하면 같지만, 엄격한 비교 연산자를 사용하면 다르다

### 문자열 검색하기

```jsx
function strTest04(){
    var str = '홍길동 이순신 유재석 강호동 홍길동'; // JS에서 문자열은 반복 가능하며 순서를 가진다
    var inputName = prompt('검색할 이름 입력');
    alert('indexOf : ' + str.indexOf(inputName)); //앞에서부터 찾아 인덱스를 반환(처음 만나는 하나만 반환)
    alert('lastIndexOf : ' + str.lastIndexOf(inputName)); //뒤에서부터 찾는다(처음 만나는 하나만 반환) 인덱스는 앞에서부터 센 인덱스를 돌려줌
}
```

- indexOf ⇒ 앞에서부터 검색해서 인덱스를 반환 (처음 만나는 하나의 값)
- lastIndexOf ⇒ 뒤에서부터 검색해서 인덱스를 반환 (처음 만나는 하나의 값)
뒤에서부터 검색하나, 인덱스는 앞에서부터 계산된 인덱스를 반환

### 문자열 추출하기

```jsx
function strTest05(){
    var strVal = '문자열 추출하기. 관련 메서드 : indexOf, substring.'
    var startIdx = strVal.indexOf(':');
    var endIdx = strVal.lastIndexOf('.');
    var result = strVal.substring(startIdx+1, endIdx); // 앞에 인덱스는 포함, 뒤의 인덱스는 바로 전까지
    alert(result);
}
```

- substring 함수 사용하여 인덱스 값 두개를 인자로 입력해서 문자열 일부 추출
⇒ 앞 인덱스 값은 포함, 뒤 인덱스 값은 바로 전까지

### 키워드 나누기

```jsx
function strTest06(){
    var prop = prompt('쉼표로 구분하여 키워드를 입력해 주세요',
                '사과,바나나,수박,참외,포도,딸기');
    var propArr = prop.split(',');
    var propRes = '';

// for문을 이용하여 일렬로 출력하기
    for (var i = 0; i < propArr.length; i++) { //i는 0부터 propArr개수보다 작을때까지 작동,
        propRes += propArr[i] + '<br>'; // i가 인덱스에 들어가서 그 값들을 계속 더하기 연산(문자열), br로 줄바꿈
        var doc = document.getElementById("key");
    }
    doc.innerHTML = propRes;
}
```

- split 함수를 이용하여 문자열을 인자 기준으로 나눠서 배열로 반환