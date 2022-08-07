# 환경 변수 설정

## Vue-Cli를 사용할 때

> 주의! Vue Cli를 사용할 때는 dotenv 라이브러리를 인스톨 해서 사용할 필요가 없다!

1. 프로젝트의 root 폴더에 .env 파일을 만들어 준다.

2. .env파일 내부에 깃에 올리지 않을 파일을 추가해 주는데, 이때 `VUE_APP_KAKAO_JAVASCRIPT_API_KEY=API_KEY` 형태로 저장해주고, 무엇보다 중요한 것은 KEY값 앞에 VUE_APP을 붙여 주어야 하는 것이다.

3. .gitignore에 .env파일을 추가해 준다.

4. 컴포넌트에서 사용할 때는, `process.env.key` 형태로 불러와서 사용한다.
