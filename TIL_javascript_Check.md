# check

- css코드

```css
<style>
        #colorbox{
            width: 320px;
            height: 320px;
            position: relative;

        }
        #red, #green, #blue, #yellow{
            width: 150px;
            height: 150px;
            border: 1px solid black;
            position: absolute;
        }
        #green {left: 160px;}
        #blue {top: 160px;}
        #yellow {left: 160px; top: 160px;}

    </style>
```

### 체크 박스 요소 가져오기

- html 코드

```html
<body>
    
    <div id="colorbox">
        <div id="red">red</div>
        <div id="green">green</div>
        <div id="blue">blue</div>
        <div id="yellow">yellow</div>
    </div>

    <div id="base">
        <form>
            <input type="checkbox" name="all" onclick="allSel(this.checked);">전체선택 <!--checked property를 이용하여 checked 값을 가져올 수 있음-->
            <br>
            <input type="checkbox" name="chk" value="red">빨강
            <br>
            <input type="checkbox" name="chk" value="green">초록
            <br>
            <input type="checkbox" name="chk" value="blue">파랑
            <br>
            <input type="checkbox" name="chk" value="yellow">노랑
            <br>
            <input type="button" value="선택" onclick="sel();">
            <input type="button" value="clear" onclick="clearAll();">
        </form>
    </div>

</body>
```

- JS 코드

```jsx
function sel(){
    var chks = document.getElementsByName('chk'); // => node list의 형태

    for (var i = 0; i < chks.length; i++) {
        if(chks[i].checked){ // 실제 타입은 위 html코드의 input 태그임 / .checked => 체크되어 있으면 true, 안되있으면 false 리턴
            document.getElementById(chks[i].value).style.backgroundColor = chks[i].value // => input된 값을 가지고 id를 찾음. 그 id를 가진 요소(아래 div태그)
        } else { 
            document.getElementById(chks[i].value).style.backgroundColor = '';
        }
    }
}

function allSel(check){ // => 전달된 값(this.checked)을 파라미터 안에 담음
    // name이 chk인 모든 요소를 가져온다
    var chks = document.getElementsByName('chk');
    // name이 chk인 모든 요소의 checked 속성값을 '반복해서' 변경한다
    for (var i = 0; i < chks.length; i++) {
        chks[i].checked = check
    }
}

function clearAll(){
    //모든 체크 해제
    allSel(false); // => 있는 함수 쓰기 allSel의 인자로 false 값 전달
    document.getElementsByName('all')[0].checked = false; //전체선택 버튼도 해제
    //배경색 없애기 (만든함수사용)
    // sel();
    // 배경색 없애기(직접)
    var colorDiv = document.querySelectorAll('#colorbox > div'); //선택자에 해당하는 요소들을 모두 가져와 배열로 반환
    for (var i = 0; i < colorDiv.length; i++) {
        colorDiv[i].style.backgroundColor ='';
    }
}
```

- sel()

  - .checked 속성을 이용해 각 체크박스가 체크 되었는 지 여부를 따져 조건문 시행
  - .checked가 true라면 반복문으로 chks의 값을 id로 가진 속성(colorbox의 자식요소)을 가지고 와서 그 값을 backgroundColor로 지정해줌

- allSel(check)

  `<input type="checkbox" name="all" onclick="allSel(this.checked);">`

  - checked property를 이용하여 checked 값을 allSel의 인자로 전달함. (this.checked) ⇒ 이 input 태그의 checked 속성
  - checked : 체크박스의 체크되어있으면 ⇒ True, 안되어 있으면 False 값 반환
  - allSel 함수가 실행되면서 chks.checked = check 로 작성하더라도 값이 대입되지 않음, for문을 이용해서 요소 하나씩의 checked 값을 바꿔 줘야함

- clearAll()

  - 모든 체크 해제를 위해 만든 함수이고, allSel의 인자로 false를 전달,
  - 전체선택 체크박스를 가져오기 위해 `document.getElementsByName('all')[0].checked = false;` Name으로 속성 가져와서 checked 값을 false로 변경
  - queryselectorAll : css선택자를 통해 해당하는 요소를 전부 가져옴
  - backgroundColor = ‘’; 으로 공백으로 지정하면 backgroundColor 값을 없애줌