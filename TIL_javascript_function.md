- 함수
    
    ## 함수 : 기능
    
    ### 명시적 함수
    
    ```jsx
    function func01(){
                alert('함수의 이름이 있습니다!');
            }
    ```
    
    ### 익명함수
    
    ```jsx
    var func02 = function(){
                alert('함수의 이름이 없어요!!');
            }
    ```
    
    - JS에서는 함수를 값으로 가질 수 있다
        - 변수 = 값
        - 함수 값을 갖는 변수를 실행시키려면 ()을 붙여줘야함.
    
    ### 즉시실행함수
    
    ```jsx
    function func03(){
                (function(){
                    alert('즉시 실행!');
                })(); 
            }
    ```
    
    - `function(){alert('즉시 실행!');}`이라는 함수를 값으로 가지고, ();를 붙여 실행하는 함수 / 함수임을 표현하기 위해 괄호로 감싼 형태
    
    ### 함수 리터럴
    
    ```jsx
    function literalprn(literal){
        literal('안녕하세요, 함수형태의 값! 입니다.');
    }
    
    function func04(){
        literalprn(function(msg){alert(msg);});
    }
    
    ```
    
    - literal 이란 인자(변수)에 값으로 익명함수(`function(msg){alert(msg)};`)가 들어감.
    - '안녕하세요, 함수형태의 값! 입니다.' 부분은 익명함수에 msg에 들어감.
    
    ### 클로저
    
    - 외부 함수에 접근 가능 (변수 등)
    
    ```jsx
    function closureFunc(val){
                var suffix = '님, 안녕하세요!';
                function innerFunc(){
                    alert(val + suffix);  //자식 함수는 부모 함수가 가진 값들을 가져와서 쓸수 있다
                }
                return innerFunc;
            }
    
            var closureTest01 = closureFunc('이성민'); 
    
            function closureTest02(val){
                closureFunc(val)();
            }
    ```
    
    ```html
    <p onclick="closureTest01();">클로저 : 외부함수에 접근 가능 (변수 등) </p>
    <p onclick="closureTest02('아이유')">클로저 : 외부함수에 접근 가능 (변수 등) </p>
    ```
    
    - closureTest01이 closureFunc함수를 값으로 가지고, closureTest01이 호출되면서 (p태그에 onclick이벤트 발생) innerFunc 함수가 생성되고 (이때 미리 지정한 val 인자 ‘이성민’이 innerFunc의 인자로 들어간다.
    - return innerFunc; (괄호가 없으므로 함수실행이 아님을 알 수 있음)으로 함수를 값으로 돌려줌.
    - closureTest02는 호출될때 val 인자를 받아 closureFunc에 전달