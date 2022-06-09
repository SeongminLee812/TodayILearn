### 객체

#### 객체의 구성

- property : 속성
- function(method) : 기능
- this : 객체 내부의 메서드나 속성을 정의
- prototype : 객체의 확장

### this

```jsx
function object01(){
		this.name = '홍길동';
		var name02 = 'hong-gd'
		this.getName02 = function(){ // 객체에 함수를 값으로 지정함. 
				return name02; // => 클로저 이용해서 외부 변수를 가져와 사용
    }
}
```

- this란 파이썬의 self처럼 객체 자기자신을 의미
- [this.name](http://this.name) ⇒ 객체에 귀속되는 name 변수
- 객체에 메서드도 지정가능

### 객체 리터럴

```jsx
var object02 = {  //파이썬의 딕셔너리와 유사 {property:value}
    name: 'kim-sd',
    prn: function(){  // 값이 함수이므로 method가 됨
        alert(object02.name + ' : 010-0000-0000');
    }
}
```

- 파이썬의 딕셔너리{key:value}와 유사하게 {property:value} 구조를 가짐
- prn의 value로 함수를 지정했기 때문에 메서드가 되고, `객체.prn`으로 메서드 불러오기 가능

### 객체를 생성하는 방법

```jsx
function objTest(){  // 클래스의 객체 생성하는 방법 1
    var obj01 = new object01(); //함수 앞에 new를 붙여서 실행하면 객체가 됨
    // alert(obj01.name);
    // alert(obj01.name02); // 이 경우 name02 변수는 object01 함수 안에서만 선언된 것으로 this로 객체에 귀속되지 않아 undefined
    // alert(obj01.getName02()); // 이 경우 클로저를 통해서 외부 변수를 가져와 사용하므로 값이 출력됨

    // alert(object02.name);
    // object02.prn();
    obj01.prn();
}
```

- 함수 앞에 new를 붙여서 var로 객체 선언

### 메서드 추가하기

```jsx
object01.prototype.prn = function(){ // prn이라는 함수를 object에 추가함
    alert(this.name + ' : 010-1111-1111');
}
```

- `함수명.prototype.새로운메서드명`코드를 이용해 새로운 메서드를 함수에 생성할 수 있다