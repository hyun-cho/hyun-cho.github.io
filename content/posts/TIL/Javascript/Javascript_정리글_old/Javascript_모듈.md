# 모듈 사용방법

- `import`와 `export`에 의존적

- 예제 파일 구조

```dir
index.html
main.js
modules/
    canvas.js
    square.js
```

- 모듈화 하고자하는 파일에서 `export`를 통해 내보낼 수 있다.
- 내보낼 항목은 최상위 항목이어야한다.

```javascript
// square.js
export { name, draw, reportArea, reportPerimeter };
```

- `import` 구문을 통해 우리가 사용할 스크립트로 가져온다.

```javascript
// main.js
import { name, draw, reportArea, reportPerimeter } from './modules/square.js';
```

- main.js 모듈을 HTML 페이지에 적용하여 모듈을 사용한다.

- `<script>` 요소에 `type="module"`을 포함시켜야 한다.

```HTML
<script type="module" src="main.js"></script>
```

- 추가적인 정보는 [여기](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Modules)서
