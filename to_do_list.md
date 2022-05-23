# to_do_list -jvs
자바스크립트로 할일목록 만드는 to_do_list를 만들어 보았다.

# HTML
먼저, !+tab을 이용해서 기본적인 HTML문서 골격을 잡아 주었다.   
title에는 To_DO_List라고 적었다.   
```<link rel="stylesheet" href="style.css">```태그를 통해 css파일과 연결해주었다.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To_Do_List</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/css/all.min.css"
    rel="stylesheet">
    <link rel="stylesheet" href="style.css">
</head>
<body>

</body>
</html>
```

```body```부분에는 todolist로 전체를 감싸는 wrapper를 만들고, 할 일을 입력하는 창을 ```input__section```으로 해주었고, 입력 후 할일들이 표시되는 부분을 ```item__list```로 해주었다.    
👉```autofocus```는 input태그에서 사용하는 속성이다. 입력창에 자동으로 포커스를 적용할 때 사용한다.    
예를 들어, **포커스를 적용한다**는 것은 우리가 어떠한 입력창에 텍스트를 입력하기 위해 마우스를 클릭하여 입력창을 선택한다. 이 과정 필요없이 바로 해당 입력창에서 텍스트를 입력할 수 있음을 의미한다.   
👉```button```은 할 일을 입력하는 창 밑에 +기호인 버튼을 의미한다.   
```html
<body>
    <div id="todolist">
        <div class="main__title">
        <h1>To do list</h1>
    </div>

    <div class="input__section">
        <form>
            <div>
                <input type = "text" class="item" autofocus="true">
            </div>
            <div>
                <button type = "button" class="input__button"><i class="fas fa-plus-circle"></i></button>
            </div>
        </form>
    </div>

        <div class="item__list"></div>
    </div>
    <script src="main.js" defer></script>
</body>
```
```body```가 끝나기 바로 전에는 자바스크립트와 연결하는 ```<script>```태그를 삽입하였고 ```defer```라는 옵션을 주었다.   
👉```defer```는 ```<script>```태그의 defer속성은 페이지가 모두 로드된 후에 해당 외부 스크립트가 실행 된다. 이 속성은 ```<script> ```요소가 외부 스크립트를 참조하는 경우에만 사용할 수 있으므로, src속성이 명시된 경우에만 사용할 수 있다.
```html
<script src="main.js" defer></script>
```

# javaScript
```javascript
'use strict';

let itemList = [];
let inputButton = document.querySelector(".input__button");
inputButton.addEventListener("click", addItem);   
```
1. 자바스크립트 코드는 ```strict``` 모드로 작성하였다. 그리고 할 일 목록들을 배열 선언해주었다.   
2. querySelector()를 사용하여 HTML에서 input__button 클래스를 가져와서 inputButton변수에 할당해주었다. (var대신 let 구문 사용)
3. addEventListener()를 사용하여 버튼을 클릭하면 addItem() 함수가 실행되도록 설정하였다.   
👉```querySelector()```는 html의 태그와 class,id 모두 javaScript에 가져오는 것이다.   
👉```addEventListener()```는 document의 특정요소(Id,class,tag 등)event(예를 들어, click하면 함수를 실행하라, 마우스를 올리면 함수를 실행하라 등)를 등록할 때 사용한다.

```javascript
function addItem() {
    let item = document.querySelector(".item").value;
    if (item != null) {
        itemList.push(item);
        document.querySelector(".item").value = "";
        document.querySelector(".item").focus();
    }

    showList();
}
```
1.  html에서 텍스트타입의 input을 받는 item 클래스를 querySelector()를 이용하여 받아오고 item변수를 할당해 주었다.
2. item이 null이 아닐 때, itemList 배열에 item을 push했고, 그러고나서 item 클래스의 value는 공백으로 즉, 지워주고 focus를 적용시켜서 커서가 깜박이게 설정하였다.   
👉```push()```는 배열의 끝에 요소를 추가하는 방법이다.   
👉```focus()``` 해당요소에 포커스를 부여하여 1) 텍스트 창의 경우, 커서를 위치시켜 바로 입력이 가능하다. 2) 버튼의 경우, 엔터 키를 눌렀을때 클릭 효과를 낸다.   

```javascript
function showList(){
    let list = "<ul>"
    for(let i=0; i <itemList.length; i++) {
        list += "<li>" + itemList[i] + "<span class='close' id=" + i + ">" + "\u00D7" + "</span></li>";
    }
    /*\u00D7= X 의미 */
    list += "</ul>";
    document.querySelector(".item__list").innerHTML = list;

    let deleteButtons = document.querySelectorAll(".close");
    for(let i=0; i < deleteButtons.length; i++) {
        deleteButtons[i].addEventListener("click", deleteItem);
    }
}

function deleteItem() {
    let id=this.getAttribute("id");
    itemList.splice(id, 1);
    showList();
}
```
1. ```showList()```함수를 만들었다.   
-> 입력창에 할 일을 입력하고, +버튼을 누르면 리스트에 할 일을 표시하는 것을 구현하기 위해서   
2. 먼저 for문을 통해 배열에 들어온 요소를 ul의 형태로 list 변수에 설정해 주었고, innerHTML 속성을 사용하여 item__list 클래스에 list를 담았다.   
👉```innerHTML```은 element에 포함된 HTML 또는 XML 마크업을 가져오거나 설정한다.
3. querySelector()을 이용해 class인 모든 element를 가져와서 deleteButtons에 저장하고, deleteButtons 배열의 각 요소를 인덱싱해서 X버튼이 클릭되면 해당 item이 삭제되도록 설정했다.   
👉```splice```함수는 지정한 인덱스(id)부터 지정한 숫자(1)만큼 요소를 삭제한다.

```javascript
let checkList = document.querySelector(".item__list");
checkList.addEventListener('click', event => {
    if(event.target.tagName === 'LI'){
        event.target.classList.toggle('checked');
    }
});
```
1. 리스트에 표시된 할 일을 클릭하면 ✔버튼이 왼쪽에 나타나면서 할 일에 줄 긋는 것을 구현하기 위해 item__list인 요소를 가지고와서 checkList 변수에 지정했다.
2. classList의 toggle 메서드를 사용해서 click 이벤트가 발생했을 때 checked 클래스가 존재한다면 제거하고, 존재하지 않으면 추가하도록 설정했다.   
👉```toggle```이란, on/off switch의 개념으로 스위치를 켰다, 껐다하는 기능을 가지고 있다.
