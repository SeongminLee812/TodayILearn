# Ajax 입력받은 값을 표로 옮기기

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>

    <script>
        function tableVal(){
            var doc = document.forms[0]; // => document 안 form 태그들을 배열로 다 가져옴 우리는 form 태그가 하나임
            var vals = [doc.id.value, doc.pw.value, doc.addr.value, doc.phone.value];
            // doc 객체(form태그)에서 name이 id인 값의 value 속성 가져오기, 배열로 지정

            // 유효성 검사
        for (var i = 0; i < vals.length; i++) {
            if (vals[i] == null || vals[i] == ''){ // || => or 역할
                alert('모든 내용을 채워주세요');

                return;  // => tableval()함수를 종료
            }
        }

            document.getElementById('addtr').appendChild(createRow(vals)); //createRow함수 실행해서 return 값 받아 인자로 넣어줌/ createRow의 인자로 위의 vals 배열을 넣어줌
        // tableVal 실행 이후에 입력한 내용을 칸에서 내용 지워지게 하기 
        var inputs = doc.querySelectorAll('table tr td input');
        for (var i = 0; i < inputs.length; i++) { 
            inputs[i].value = '';
        }
        }

        function createRow(vals){
            var tr = document.createElement('tr');

            for (var i in vals) {
                var td = document.createElement('td');
                td.textContent = vals[i];
                tr.appendChild(td);
            }

            var deleteTd = document.createElement('td');
            deleteTd.innerHTML = '<input type="button" value="삭제" onclick="delRow(this)">' // 여기서 this는 input 태그임(자기 자신)
            tr.appendChild(deleteTd) ;

            return tr;
        }

        function delRow(ele){ // => 인자로 this가 들어올 것임(input 태그)
            if (confirm('삭제하시겠습니까?')) {
                var deleteTr = ele.parentNode.parentNode; // ele(자기자신)의 부모요소인 td의 부모요소인 tr
                var tbody = document.getElementById('addtr');
                tbody.removeChild(deleteTr)

            }
        }

        function deleteAllRow(){
            if (confirm('전부 삭제하시겠습니까?')) {
                var tbody = document.getElementById('addtr');
                while (tbody.hasChildNodes()) { // 자식노드를 가지고 있는지 없는지 t/f로 반환
                tbody.removeChild(tbody.lastChild) 
                }
            }
        }

    </script>
</head>
<body>
    
    <form>
        <table id="intable">
            <tr>
                <td>아이디:</td>
                <td><input type="text" name="id"></td>
            </tr>
            <tr>
                <td>비밀번호:</td>
                <td><input type="text" name="pw"></td>
            </tr>
            <tr>
                <td>주소:</td>
                <td><input type="text" name="addr"></td>
            </tr>
            <tr>
                <td>전화번호:</td>
                <td><input type="text" name="phone"></td>
            </tr>
        </table>
        <input type="button" value="추가" onclick="tableVal();">
        <input type="button" value="삭제" onclick="deleteAllRow();">
    </form>

    <div id="addtable">
        <table border="1" id="ctb">
            <colgroup>
                <col width="100px">
                <col width="100px">
                <col width="300px">
                <col width="200px">
                <col width="100px">
            </colgroup>
            <thead>
                <tr>
                    <th>아이디</th>
                    <th>비밀번호</th>
                    <th>주소</th>
                    <th>전화번호</th>
                    <th>삭제</th>
                </tr>
            </thead>
            <tbody id="addtr">

            </tbody>
        </table>

    </div>

</body>
</html>
```

- if (confirm(’내용’)) { } : confirm 내용 안의 내용을 alert 띄우고 확인, 취소 값 받아서 T, F로 돌려줌
  - T면 실행문 실행
- 유효성 검사 : 정상적으로 입력이 되었는 지 확인
- `var vals = [doc.id.value, doc.pw.value, doc.addr.value, doc.phone.value];` ⇒ doc 객체(form태그)에서 name이 id인 값의 value 속성 가져오기, 배열로 지정