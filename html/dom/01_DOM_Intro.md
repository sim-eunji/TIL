# 1장. 노드 개요 

### 1.1 문서 모델 객체 (DOM)은 자바스크립트 Node 개체의 계층화된 트리다.

브라우저는 HTML 문서를 로딩 시 계층 구조를 해석해서 트리 형태로 구조화된 노드를 가지고 있는 문서(DOM)를 생성한다. 

DOM의 목적은 JavaScript 를 사용해서 이 문서에 대한 스크립트 작성을 위한 프로그래밍 인터페이스를 제공하는 것이다. 

### 1.2 노드 개체 유형

- HTML을 다룰 때 마주치게 되는 일반적인 노드 유형
    - DOCUMENT_NODE : window.document
    - ELEMENT_NODE : `<body>, <a>, <p>, <script>, <style>, <html>, <h1> ` ,, 
    - ATTRIBUTE_NODE : class="funEdges"
    - TEXT_NODE : HTML 문서 내의 텍스트 문자
    - DOCUMENT_FRAGMENT_NODE : document.createDocumentFragment()
    - DOCUMENT_TYPE_NODE : <!DOCUMENT html>

위 목록의 노드 유형 형식은 JavaScript 브라우저 환경에서 Node 개체의 속성으로 기록되는 상수 값의 속성과 동일하다.

[노드 인터페이스/생성자와 인스턴스에 부여되는 관련 숫자 분류 값 및 이름 ](https://www.notion.so/fcbfe1638da14e02bb2adefac9966f2a)

### 1.3 Node 개체로부터 상속받은 하위 노드 개체

통상적인 DOM 트리의 각 노드 개체는 Node 로부터 속성과 메소드를 상속받는다.  

- Object < Node < Element < HTMLElement ,,,

→ 모든 노드가 prototype 체인의 속성뿐만 아니라 생성자로부터 일련의 기본 속성 및 메서드를 상속 받는다. 

### 1.4 노드를 다루기 위한 속성 및 메서드

앞서 말한 것 처럼 모든 노드 개체(Element, Attr, Text 등)는 속성과 매서드를 Node 개체로부터 상속받는다. 

노드 인터페이스에서 제공되는 속성 및 메서드 외에 document, HTMLElement, HTML*Element 인터페이스와 같은 하위 노트 인터페이스에도 수많은 관련 속성 및 메소드가 제공된다. 

주로 사용되는 것은 Node 속성 및 메서드 인데, 하위 노드를 다루기 위한 관련 속성도 포함되어 있다. 

- Node 속성
    - childNodes
    - firstChild
    - lastChild
    - nextSibling
    - nodeName
    - nodeType
    - nodeValue
    - parentNode
    - previousSibling

- Node 메서드
    - appendChild()
    - cloneNode()
    - compareDocumentPosition()
    - contains()
    - hasChildNodes()
    - insertBefore()
    - isEqualNode()
    - removeChild()
    - replaceChild()

- Document 메서드
    - document.createElement()
    - document.createTextNode()

- HTML*Element 속성
    - innerHTML
    - outerHTML
    - textContent
    - innerText
    - outerText
    - firstElementChild
    - lastElementChild
    - nextElement
    - previousElementChild
    - children

- HTML element 메서드
    - insertAdjacentHTML()

### 1.5 노드의 유형과 이름 식별하기

모든 노드는 Node로부터 상속받는 nodeType과 nodeName 속성을 가진다. 

다음은 흔히 사용되는 노드들의 숫자 값이다. 

노드 유형을 판별하는 것은 해당 노드에서 사용 가능한 속성과 메서드를 알 수 있게 해준다. 

```jsx
/* 
	1. Node.DOCUMENT_TYPE.NODE === 10 
*/

console.log(
	document.doctype.nodeName,
	document.doctype.nodeType
)
// 'html',10  document.doctype => <!DOCUMENT html>

/*
	2. Node.DOCUMENT_NODE === 9 
*/

console.log(
	document.nodeName,
	document.nodeType
)
// '#document', 9

/*
	3. Node.DOCUMENT_FRAGMENT_NODE === 11
*/

console.log(
	document.createDocumentFragment().nodeName,
	document.createDocumentFragment().nodeType
)
// '#document-fragment', 11

/*
	4. Node.ELEMENT_NODE === 1
*/
console.log(
	document.querySelector('a').nodeName,
	document.querySelector('a').nodeType
)
// 'A', 1

/*
	5. Node.TEXT_NODE === 3
*/
console.log(
	document.querySelector('a').firstChild.nodeName,
	document.querySelector('a').firstChild.nodeType,
)
// '#text', 3
```

### 1.6 노드 값 가져오기

nodeValue 속성은 Text 와 Comment 를 제외한 대부분 노드 유형에서는 null 값을 반환한다. 

이 속성의 용도는 실제 텍스트 문자열을 추출하는데 초점을 맞추고 있다. 

```html
<body>
	<a href="#"> 하이 </a>
</body>
<script>
	console.log(document.doctype.nodeValue)
	console.log(document.nodeValue)
	console.log(document.createDocumentFragment().nodeValue)
	console.log(document.querySelector('a').nodeValue)
	// 전부 null 출력

	console.log(document.querySelector('a').firstChild.nodeValue);
	// '하이'
</script>
```

### 1.7 JavaScript 메서드를 사용해서 Element 및 Text 노드를 생성하기

브라우저는 HTML 문서를 초기 로딩할 때 노드 생성을 처리하지만, JavaScript 를 사용해서 직접 노드를 생성하는 것도 가능하다. 

- createElement()
- createTextNode()

위와 같은 메서드로 노드를 간단하게 만들 수 있다. 

### 1.8 JavaScript 문자열을 사용하여 DOM에 Element 및 Text 노드를 생성 및 추가하기

innerHTML, outerHTML, textContent, insertAdjacentHTML() 속성 및 메서드는 문자열을 사용하여 노드를 생성하고 추가하는 기능을 제공한다. 

```html
<body>
	<div id="a"> </div>
	<div id="b"> </div>
	<span id="c"> </span>
	<div id="d"> </div>
	<div id="e"> </div>
</body>

<script>
	// 1. strong 요소와 text 노드를 생성해 DOM에 추가 
	document.getElementById('a').innserHTML = '<strong>Hi</strong>'

	// 2. div element와 text 노드를 생성해서 <div id="b">와 교체한다.
	document.getElementById('b').outerHTML = '<div id='b' class='new'>outerHTML</div>'

	// 3. text 노드를 생성해서 div#c 를 갱신한다. 
	document.getElementById('c').textContent = 'dude'
	
	// 4. text 노드를 생성해서 div#d 를 갱신한다. 
	document.getElementById('d').innerText = 'textContent test'

	// 5. text 노드를 생성해서 div#e 를 text 노드로 바꾼다. 
	document.getElementById('e').outerText = 'outerText'
</script>
```

### 1.9 DOM 트리의 일부를 JavaScript 문자열로 추출하기

DOM에 노드를 생성하고 추가하는 데 사용하는 것과 동일한 속성들(innerHTML, outerHTML, textContent)은 

DOM의 일부를 문자열로 추출하는 데 사용한다. 

```html
<body>
	<div id="a">하이</div>
	<div id="b"> Dude <storng>!</strong> </div>
</body>

<script>
	console.log(document.getElementById('a').innerHTML) // '하이'
	console.log(document.getElementById('a').outerHTML) // <div id="a">하이</div>

	console.log(document.getElementById('b').textContent) // Dude!

	console.log(document.getElementById('b').innerText) // Dude!
	console.log(document.getElementById('a').outerText) // Dude!  
</script>
```

### 1.10 appendChild() 및 insertBefore() 를 사용하여 노드 개체를 DOM에 추가하기

appendChild() 및 insertBefore() 노드 메서드는 노드 개체를 DOM 트리에 삽입할 수 있게 해준다. 

- appendChild() : 노드가 호출된 노드의 자식 노드 끝에 삽입할 수 있게 해준다.
- insertBefore() : 끝에 노드를 추가하는 것 외에 삽입 위치를 조정하는 것이 필요할 때 사용한다.

    삽입될 노드와 해당 노드를 삽입하고자 하는 문서내의 참조 노드다. 

### 1.11 removeChild() 및 replaceChild() 를 사용하여 노드를 제거하거나 바꾸기

- DOM에서 노드를 제거할 때 삭제하고자 하는 노드를 선택하고, 부모 노드에 대한 접근을 얻는다. (parentNode 속성)

    → 부모 노드에서 삭제할 노드에 대한 참조를 전달하여 removeChild() 메서드를 호출한다.

```html
<body>
	<div id="a">Hi</div>
	<div id="b">dude</div>
</body>

<script>
	// element 요소를 삭제
	const divA = document.getElementById("a")
	divA.parentNode.removeChild(divA)

	// 텍스트 노드를 삭제 
	const divB = document.getElementById("b").firstChild // dude
	divB.parentNode.removeChild(divB)

</script>
```

### 1.12 cloneNode()를 사용하여 노드를 복제하기

cloneNode() 메서드를 사용하여 단일 노드 혹은 모든 자식 노드를 복제할 수 있다. 

```html
<body>
	<ul>
		<li>Hi</li>
		<li>Hi there</li>
	</ul>
</body>

<script>
	const cloneUL = document.querySelector('ul').cloneNode()
	console.log(cloneUL.constructor) // HTMLUListElement()가 출력
	console.log(cloneUL.innerHTML) // 빈 문자열 출력 
</script>
```

노드와 그 자식을 모두 복제하려면, cloneNode(true)로 전달한다. 

### 1.13 노드 컬렉션(NodeList, HTMLCollection)에 대한 이해