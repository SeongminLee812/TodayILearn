# DOM

- Document Object Model
- text형식으로 전달하기 위해 문서를 object로 표현하는 모델

[DOM 소개 - Web API | MDN](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction)

## 방법

### querySelector

```jsx
function searchQS(){
    var doc = document.querySelector("#test");
    doc.innerHTML = 'querySelector("css선택자")';
}
```

- css선택자를 이용해 element 선택

### getElementById

```jsx
function searchId(){
    var doc = document.getElementById("test");
    doc.innerHTML = 'id로 탐색!';
    doc.style.color = 'red';
}
```

- id로 탐색하기
- id는 고유한 것이므로 중복된 값이 없어 Element 단수형으로 사용

### getElementsByName

```jsx
function searchName(){
    var doc = document.getElementsByName('test02');
    doc[1].value = 'name 속성으로 엘리먼트 탐색!';
}
```

- Elements로 속성들을 가져오면 배열(파이썬의 리스트)로 값을 리턴해줌, 값이 없으면 빈 리스트를 반환
- JS에서 배열은 파이썬 리스트와 같은 방법으로 인덱싱 가능

### getElementsByTagName

```jsx
function searchTagName(){
		var doc = document.getElementsByTagName('p');
		    doc[3].innerHTML = 'tagname으로 탐색!!';
		    doc[3].style.color = 'blue';
}
```

- TagName으로 p 태그를 탐색해서 전부 배열에 담음. 아래 html 태그에서 4번째 값인 doc[3]에 값 변경

### HTML 코드

```jsx
<p onclick="searchQS();">DOM 탐색 메서트</p>

    <p id="test" onclick="searchId();">1. 엘리먼트의 id로 탐색 : 엘리먼트 하나를 선택(중복 불가) -> 반환 : 값 하나</p>

    <p onclick="searchName();">2. 엘리먼트의 name으로 탐색 : 엘리먼트 여러개를 선택 -> 반환 : 배열(NodeList)
        <br><input type="text" name="test02">
        <br><input type="text" name="test02">
        <br><input type="text" name="test02">
    </p>

    <p onclick="searchTagName();">3. 엘리먼트의 tag name으로 탐색 : 엘리먼트 여러개를 선택 -> 반환 : 배열(NodeList)</p>
```

<aside>
💡 표기법
카멜 표기법 : 새로운 단어가 나오면 대문자, 나머지는 소문자(Java, JS는 전부 카멜표기법)
스네이크 표기법 : 단어와 단어 사이에 _

</aside>