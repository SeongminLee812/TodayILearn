# window

```jsx
<style type="text/css">
    #regist {
        border: 1px solid black;
        background: pink;
        position: absolute;
        top: 200px;
        left: 500px;
        display: none;
    }
</style>

<!-- 
    display : none;			//공간을 차지하지 않는다 (만들어지지 않은 상태)
    visibility: hidden;		//공간을 차지하고, 보이지 않는다 (만들어져 있는 상태)
 -->

<script type="text/javascript">
    function openWin() {
        var url = "js13-window-popup.html";
        var title = "myframe";
        var prop = "top=200px,left=600px,width=500px,height=500px";
        window.open(url, title, prop); //=> title 속성은 팝업창이 열릴 위치 지정 (target)
    }

    function registForm() {
        document.getElementById("regist").style.display = "block"; // => display:none 에서 만들어지지 않은 상태에서 이 부분에서 만들어짐
        document.body.style.background = "gray";

        var btns = document.getElementsByName("btn");
        for ( var i in btns) { // btns가 가지고 있는 property를 전부 가져옴
	// for (var i = 0; i < btns.length; i++) 과 같다.
            btns[i].disabled = "disabled"; // 불린 으로 구성되고 컨트롤이 가능한지 여부/ disabled는 클릭이 불가능
        }
    }

    function closeWin() {
        document.getElementById("regist").style.display = "none";
        document.body.style.background = "white"; // backgroundColor를 원래대로

        var btns = document.getElementsByName("btn");
        for ( var i in btns) {
            btns[i].disabled = ""; //disabled 속성 없애기
        }
    }

    function idchk() {
        alert("중복체크를 확인하세요");
    }

    function idCheck() {
        open("js13-window-id-check.html", "", "width=300px,height=300px");
    }
</script>
<body>
    
    <h1>window객체</h1>
	
	<pre>
	프로퍼티
	 -document <!--창이 포함한 문서의 참조를 반환합니다.-->
	 -history <!--읽기 전용 속성은 History 객체로의 참조를 반환합니다. History 객체는 브라우저의 세션 기록(현재 페이지를 불러온 탭 혹은 프레임이 방문했던 페이지)을 조작할 때 사용합니다.-->
	 -location <!--프로퍼티에 접근하면 읽기 전용인 Location 오브젝트를 얻어올 수 있습니다. 이는 현재 도큐먼트의 로케이션에 대한 정보를 담고 있습니다.-->
	 -navigator <!--읽기 전용 속성은 스크립트를 구동 중인 애플리케이션에 대한 메서드와 속성을 가진 Navigator 객체의 참조를 반환합니다. navigator : 유저에이전트의 신원과 상태-->
	 -screen <!--현재 윈도우 창이 렌더링되는 스크린 속성을 나타내는 property-->
	 -frames <!--현재 윈도우의 서브프레임들을 배열형태로 나타댐, 인덱스와 length 사용 가능 frame은 html 요소--> <!--frame은 다른 html 요소를 나타낼 수 있는 지역 요소, 권장되지 않음-->
	 -parent <!--현재 윈도우나 서브프레임의 부모속성을 나타냄. 부모요소가 없으면 자기 자신.-->
	 -top <!--가장 상위 윈도우 레퍼런스, 가장 부모요소/ parent는 바로 위 부모요소-->
	 -self <!--읽기전용, 자기 자신을 windowproxy로 반환 var w1 = window;
		var w2 = self; => 이거는 작동 X
		var w3 = window.window;
		var w4 = window.self; w1, w3, w4는 똑같다-->
	메서드
	 -alert()
	 -confirm()
	 -prompt()
	 -back()
	 -forward()
	 -setInterval()
	 -clearInterval()
	 -setTimeout()
	 -open()
	 -close()
	 -scroll(),scrollBy(), scrollTo() 
	</pre>

    
	<div id="f1">
		<h1>팝업창 만들기</h1>
		<button onclick="openWin()" name="btn">창열기</button>
		<hr>
		<h1>회원가입하기(div팝업창)</h1>
		<button onclick="registForm()" name="btn">회원가입</button>
	</div>

	<div id="regist">
		<form>
			<table>
				<caption>회원가입</caption>
				<tr>
					<td>아이디</td>
					<td><input type="text" name="id" onclick="idchk()" readonly="readonly" /> 
					<input type="button" value="중복체크" onclick="idCheck()" /></td>
				</tr>
				<tr>
					<td>패스워드</td>
					<td><input type="password" name="pwd" style="color: red;"/></td>
				</tr>
				<tr>
					<td colspan="2" align="center"><input type="button" value="확인" onclick="closeWin()" /></td>
				</tr>
			</table>
		</form>
	</div>

	<br/><br/><br/>
	<iframe name="myframe"></iframe> <!--문서 내부에서 보여주는 '새로운' 문서임-->

</body>
```

- style
  - display : none ; 공간이 만들어지지 않음
  - visiblity : hidden; 공간은 만들어졌으나 보이지 않는 상태
- window.open(url, title, prop)
  - url : 팝업창의 주소
  - title : target이며 팝업창이 열릴 창
  - prop : 팝업창의 속성
- disabled : button의 속성값이며 disabled : disabled;가 되면 클릭 불가능
- iframe :  팝업창을 여는 방법 중 하나로써, html 문서 내에 다른 문서를 와서 염. ⇒ 서로 다른 문서임. map등을 삽일할 때 사용 ⇒ not recommended

```jsx
<script type="text/javascript">
    var ids=["multi","java","script"]; //=> 데이터 베이스라고 생각하자
    
    function confirmChk(){
        var id=document.getElementsByName("id")[0].value; // 입력받은 값
        var div=document.getElementsByTagName("div")[0];
        if(ids.indexOf(id)!=-1){ // id가 ids에 없지 않다면 = 있다면
            div.innerHTML="<b>아이디가 존재합니다.</b>";
        }else{
            var userId="사용할 수 있는 아이디입니다."
                    +"<input type='button' value='확인'"
                    +"onclick='insertId(\\""+id+"\\")'>"; // => "를 그대로 표현하기 위해 \\ 사용 => id의 값을 문자열로 입력해야 해서 
            div.innerHTML=userId;          
        }
    }
    
    function insertId(id){
        opener.document.getElementsByName("id")[0].value=id; // => opener : open 시켜준 객체 js13-window.html => parent임
        opener.document.getElementsByName("pwd")[0].focus(); // fucus는 커서를 포커싱하는 것
        close(); // self.close(); 앞에 self가 숨어있음
    }
</script>
<table>
		<tr>
			<td>아이디</td>
			<td><input type="text" name="id"/></td>
		</tr>
		<tr>
			<td colspan="2">
				<input type="button" value="중복확인" onclick="confirmChk()"/>
				<input type="button" value="취소" onclick="self.close()"/>
			</td>
		</tr>
	</table>
	<div></div>
```

- opener : 해당 팝업창을 열게해준 객체,
  - `opener.document.getElementsByName("id")[0].value=id;` ⇒ opener인 상위 html 문서의 이름이 id인 요소의 value값에 전달
- .focus : 커서를 이동시켜줘서 키보드 입력하면 바로 그곳에서 입력되게 해줌