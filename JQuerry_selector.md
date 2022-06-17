# JQuerry 기본문법 및 selector



## 기본문법



```jsx
<script src="./resources/js/jquery-3.6.0.min.js"></script> 
    <!--JQuery 파일 다운로드 받아서 폴더에 넣어놓고 스크립트 연결
    compressed 파일 : 공백없어서 용량작음, uncompressed 파일 : 공백 있어서 용량 크나 알아보기 쉬움(수정할일 있으면)
    위 스크립트 태그 안에 작성하면 안되고 따로 스크립트 태그 만들어서 함수 생성해야함-->

<script>
/*
    jquery 기본 작성법 : css selector
    jquery('#id').method();
    $('css selector').method();
    // ex : $('p').css('color', 'red');
    
    //javascript
    window.onload=function(){}

    //jquery (window.onload=function(){} 만들기)
    // 1.
    $(document).ready(function(){

    });

    // 2.
    $(function(){

    });
*/

$(function(){
    $('#test-btn').click(function(){
        alert('클릭했음!');
    });
    /*
        document.getElementById('test-btn').onclick = function(){
            alert('클릭했음!')
        }
    */
});

function showImg(){
    $('img').show();
}
function resizeImg(){
    // $('img').css('width', '100px').css('height', '100px')
    $('img').css({'width':'50px', 'height':'50px'}) // 객체리터럴 사용
}
function addImg(){
    $('img').last().after('<img src="resources/image/img01.jpg">')
}
function toggleImg(){
    $('img').toggle();
}
<body>

    <h1>JQurey : Javascript Library</h1>

    <button id="test-btn">클릭미</button>
    <br>
    <button onclick="showImg();">이미지 보이기</button>
    <button onclick="resizeImg();">이미지 축소</button>
    <button onclick="addImg();">이미지 추가</button>
    <button onclick="toggleImg();">이미지 보이기/숨기기</button>
    <br>
    <br>
    <div>
        <img src="resources/image/img01.jpg" alt="test">
    </div>
    
    
</body>
```

- JQurey 기본 작성법
  - css selector 이용
  - jquery(’#id).method()
  - $(’css selector’).method()
- window.onload=function(){}의  jqurey 표기
  - `$(document).ready(function(){});`
  - `$(function(){});`
- last() : 선택자 중 마지막 요소
- after(내용) : 선택된 요소 다음에 인자 삽입



# selector

```jsx
$(document).ready(function(){
        $('div:eq(0)').css({'border':'1px solid red', 'width':'400px', 'height':'200px'});
        $('#view').css({'border':'1px solid red', 'width':'400px', 'height':'100px'})
    })

    function a1(){
        $('span').css('color', 'skyblue');
        $('#view').text("$('span').css('color', 'skyblue');");
    }
    function a2(){
        $('#t1').css('color', 'dodgerblue');
        $('#view').text("$('#t1').css('color', 'dodgerblue');");
    }
    function a3(){
        jQuery('.t2').css('color', 'orange');
        jQuery('#view').text("jQuery('.t2').css('color', 'orange');");
    }
    function a4(){
        $('li span').css({'background-color':'yellow'}); // => 자식요소에 있는 모든 자식요소 포함
        $('#view').text("$('li span').css({'background-color':'yellow'});");
    }
    function a5(){
        $('li > span').css('background-color', 'aquamarine'); // => 바로 아래 자식요소만 포함
        $('#view').text("$('li > span').css('background-color', 'aquamarine');");
    }
    function a6(){
        $('li:nth-child(6)').css('background-color', 'lime'); //=>li 태그 중 6번째 태그(1부터 시작) odd: 홀수 even:짝수 선택
        $('#view').text("$('li:nth-child(6)').css('background-color', 'lime');");
    }
    function a7(){
        $('li:first-child').css('background-color', 'violet'); //=>li태그 중 첫번째 태그
        $('#view').text("$('li:first-child').css('background-color', 'violet');");
    }
    function a8(){
        $('li:last-child').css('background-color', 'black');
        $('#view').text("$('li:list-child').css('background-color', 'red');");
    }
<div>
    <ul>
        <li><span>tag로 선택</span></li>
        <li id="t1">id로 선택</li>
        <li class="t2">class로 선택</li>
        <li><span>parent child로 선택</span></li>
        <li><b><span>parent &gt; child로 선택</span></b></li>
        <li>:nth-child(n/odd/even)로 선택</li>
        <li>:first-child로 선택</li>
        <li>:last-child로 선택</li>
    </ul>
</div>
<br>
<div>
    <button onclick="a1();">태그선택(span)</button>
    <button onclick="a2();">id선택(t1)</button>
    <button onclick="a3();">class선택(t2)</button>
    <button onclick="a4();">p c 선택</button>
    <button onclick="a5();">p &gt; c 선택</button>
    <button onclick="a6();">nth-child 선택</button>
    <button onclick="a7();">first-child 선택</button>
    <button onclick="a8();">last-child 선택</button>
</div>

<h2>코드내용</h2>
<div id="view"></div>
```

- javascript의 querySelectorAll과 비슷한 느낌으로 생각하자
- $(’tagName’) : tag로 선택
- $('idName') : id로 선택
- jQuery('.className') : class로 선택
- $('li span') : parent child로 선택 ⇒ 모든 하위(자손)요소
- $('li > span') : parent > child로 선택 ⇒ 모든 자식 요소(1촌)
- $('li:nth-child(6)') :nth-child(n/odd/even)으로 선택 ⇒ 여기서 n은 인덱스와 다르게 첫번째 요소가 1부터 시작함
- $('li:first-child') :first-child로 선택 ⇒ 요소의 부모의 첫번째자식들( li:first-child ⇒ li의 부모의 첫번째 li태그)
  - .first()는 하나의 요소만 탐색하지만 :first-child는 하나 이상의 요소 탐색
  - :nth-child(1)과 같다
  - https://api.jquery.com/first-child-selector/#entry-examples
- $('li:last-child') : :last-child로 선택 ⇒ 요소의 부모의 마지막 자식