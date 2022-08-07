# 카카오 로그인

1. 다음 코드를 index.html에 추가해준다.
   `<script src="https://developers.kakao.com/sdk/js/kakao.js"></script>`

2. 다음 코드를 컴포넌트나 뷰 페이지에 삽입한다.
   
   - `Kakao.init('API_KEY')`
   
   - 1번에서의 코드와 같은 페이지에 init하는 코드가 위치하지 않으므로, eslint에서 에러를 뱉어내지만, 정상동작한다. 같은 index.html에 넣으려 했지만 환경변수를 읽어오는 process.env가 index.html에서는 작동하지 않았기 때문에 컴포넌트에 넣어 실행시켰다.
   
   - 삽입할 view나 컴포넌트에 주의해야 한다. 만약 컴포넌트에 위 코드를 삽입했을 때, 같은 view 상에서 다른 컴포넌트로 넘어갔다 위 컴포넌트로 다시 이동해 위 코드가 반복적으로 실행될 경우, 위 코드가 이미 실행되었다는 오류가 출력되며 페이지가 이동되지 않는다. 따라서, 재렌더링되지 않는 컴포넌트 혹은 view에 위 코드가 삽입되어야 한다.
   
   - 컴포넌트에 삽입할 경우 렌더링 관련 오류가 날 수도 있다. 이럴 때는 onMounted 사용을 고려해 보자

3. `console.log(Kakao.isInitialized())`를 찍어 정상 동작을 확인해 보자
