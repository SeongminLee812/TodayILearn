# 배열 Array

### 배열 만들기

```jsx
var arrObj = new Array(); // 생성자 사용해서 만드는 방법
        var arrObj2 = ['v1', 'v2', 3, 4]; // 대괄호 이용해서 만드는 방법

        // alert(typeof(arrObj)); //object
        // alert(typeof(arrObj2)); // object => 배열은 어떻게 만들어도 객체다

        var arrObj3 = new Array(5); // => 5칸 짜리 배열이 만들어짐 [ , , , , ]
        // alert(arrObj3[0]); // 빈 리스트가 나오는 것이 아닌 undefined가 나옴

        var arrObj4 = new Array(1, 2, 3, 4, 5); // 인자를 여러개 받으니 배열값이 생성됨
        // alert(arrObj4); [1, 2, 3, 4, 5]
        // alert(arrObj4[2]); [3]
```

- new Array 생성자 이용해서 만드는 방법
  - 위와 같이 생성한 배열의 타입은 object
  - Array의 인자를 하나만 입력하면 그 숫자만큼의 칸을 가진 배열이 만들어지고, `var arrObj3 = new Array(5)` → 5칸짜리 빈 배열
  - `arrObj3[0] => undefined 빈 리스트가 아닌 undefined로 반환`
  - `var arrObj3 = new Array(5, 6, 7, 8)` 인자를 여러개 받으면  배열의 값이됨
- var 객체명 = [ 요소 ] 로 만드는 방법
  - 파이썬 리스트 만드는 방법과 유사하게 변수 선언으로 만들 수 있음

### 다중배열

```jsx
function multiArr(){
    var arrLen = 3;
    var arr = new Array(arrLen); //3칸짜리 배열을 생성
    for (var i = 0; i < arr.length; i++) {
        arr[i] = new Array(arrLen); // 각 배열 값에 다시 3칸짜리 배열 넣기
    } 
    arr[0][0] = '수박';
    arr[0][1] = '키위';
    arr[0][2] = '포도';

    arr[1][0] = 1;
    arr[1][1] = 2;
    arr[1][2] = 3;

    arr[2][0] = ['js', 'jq'];
    arr[2][1] = ['javascript', ['js', 'jquery']];
    arr[2][2] = ['python', 'django'];

        // 배열 안 3출력
    alert(arr[1][2])
        //배열 안 jquery 출력
    alert(arr[2][1][1][1])
}
```

- 반복문 이용하여 배열 안에 배열 만들기 가능
- 파이썬 리스트 내용 변경과 같이 배열 내용 변경할 수 있으며, 배열 안에 배열도 가능하다.

### join함수

```jsx
function joinTest(){
    var arr = [1, 2, 3, 4, 5];
    var res = arr.join('+'); // => String이 됨
    alert(eval(res)); // eval() 함수는 문자열을 연산해준다...
    // (명령어를 다 실행시켜주는 함수기 때문에 해킹 등 위험이 있다 사용 지양)
}
```

- 배열.join(’’) 함수로 join인자를 배열 값 사이에 넣어주며 문자열로 변환
- eval 함수는 문자열의 명령어를 전부 실행. ⇒ 사용 지양

### sort() 문자 정렬

```jsx
function sortTest01(){
    var arr = ['a', 'd', 'b', 'e', 'c']
    arr.sort();
    alert(arr);
    // 아스키코드 순 정렬
}
```

- 아스키 코드 순으로 배열 내 문자열을 정렬

### sort() 숫자 정렬

```jsx
function sortTest02(){
    var arr = [1, 3, 2, 22, 11, 10, 8, 9];
    arr.sort(); //아스키 코드 기준이기에 [1,10,11,2,22,3,8,9]로 정렬됨
    arr.sort(compareNum);
    alert(arr); 
}
// 크기 비교 함수 사용해서 sort()
function compareNum(a, b){
    return a - b;
} 
/*  a-b가 
    양수 : 
    0 : 
    음수 :   각각 어떤게 더 크다고 나오는 지 */
```

- 그냥 sort만 하면 수의 크기로 정렬되는 것이 아닌 아스키 코드 순으로 정렬,
- compareNum 함수 만들어줘서 sort의 인자로 넣어주면 수 크기 순

[Array.prototype.sort() - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

### reverse() 뒤집기

```jsx
function reverseTest(){
    var arr = [33, 28, 11, 7, 13, 27];
    //arr.reverse(); => 단지 배열 작성 순서를 거꾸로 가는 것 뿐임
    // arr.sort(compareNum);
    // arr.reverse(); => 정렬 후 뒤집기
    // alert(arr);
    arr.sort(function(a, b){return b - a}); // 함수리터럴, 함수가 값으로 전달되서 사용됨
    alert(arr);
}
```

- sort(compareNum) 후 reverse()
- sort의 인자로 함수 값 입력(함수리터럴)

### 배열 저장 방식

```jsx
function pushAndPop(){
    var queue = new Array(); //큐 자료구조

    queue.push('first'); // => push : 뒤에다 계속 붙여주는 함수 
    queue.push('second');
    queue.push('third');
    alert(queue); 

    var a = queue.shift(); // 먼저 들어온 값이 나감, second가 0번지로 들어온다.(땡겨짐)
    console.log(a); //선입선출
    //alert(queue); => ['second', 'third']

    var b = queue.pop(); // 제일 마지막에 들어온 값이 나감
    console.log(b); // third
    alert(queue) //=> ['second']
}
```

- push() ⇒ 뒤에다 계속 붙여주는 함수
- shift() ⇒ 왼쪽에 있는 값이 나감 (나간 후 원래 배열에서 삭제)
- 큐 자료구조 ⇒ push()와 shift()를 이용해 구현
- pop() 제일 뒤의 값이 빠지고 사라짐

### slice

```jsx
function sliceTest(){
    var arrayTest01 = new Array(1, 2, 3, 4, 5, 6, 7);
    var array01 = arrayTest01.slice(1, 3); // 1이상 3미만 => 내가 코드 짤때도 이 양식에 맞추자
    // alert(array01);

    var arrayTest02 = new Array(4);
    arrayTest02[0] = new Array(1, 2);
    arrayTest02[1] = new Array(3, 4);
    arrayTest02[2] = new Array(5, 6);
    arrayTest02[3] = new Array(7, 8);
    var array02 = arrayTest02.slice(1, 3);
    // alert(array02); // 출력은 3, 4, 5, 6으로 되지만 실제로는 [3, 4], [5, 6] 인 상태이다
    array02[0][0] = 33;
    alert(array02);
    alert(arrayTest02); // 이 값도 바뀌어서 33이 들어왔다.
    // 슬라이스로 가지고 오면 서로 주소는 다르나, 같은 메모리에 저장된 같은 객체이므로 같은 내용물을 갖고 있다
    // 얕은 값 복사 : 이미 만들어진 객체 하나를 가지고 서로 가지고 논다.
    // 만약 영향을 미치지 않으려면 슬라이스한 값을 새로운 배열객체에 만들어서 지정해야 한다
}
```

- `배열객체.slice(1, 3)` 으로 인덱스 1이상 3 미만까지의 배열을 가져와 다른 배열 지정
- JS에서 슬라이스는 얕은 값 복사로 이미 만들어진 메모리에서 가져오는 것이므로 값 변경은 서로 영향을 미친다.
- 영향을 미치지 않으려면 슬라이스 한 값을 새로 배열 객체에 지정해야함.