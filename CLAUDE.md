# 소리셈 놀이터 (calcul)

문제를 소리로 읽어 주고 말이나 손글씨(숫자 캔버스)로 답하는 더하기·빼기·구구단 놀이.

## 구조
- `index.html` 하나(약 50KB). 섹션: `#vHome`(시작·설정) → `#vPlay`(문제·마스코트·입력) → `#vResult`(결과), `#vIdeas`("셈이 놀이터에 넣은 아이디어" 미리보기 안내). 제목 `#brand`.
- 읽어 주기는 `speechSynthesis`(한국어 음성 선택), 설정 저장은 `store.get/set`(localStorage, try/catch).
- 음성 인식(말로 답하기)은 실시간 표시·자동 재시도·숫자 해석을 지원한다. 숫자 해석 규칙(한글 수사 "십이", "열둘" 등)을 바꿀 때는 구구단·큰 수도 같이 확인. 긴 문장은 숫자로 읽지 않는다.
- 글자·아래 버튼을 일부러 크게 키운 상태(1.5배)다. 낮은 화면에서 입력 영역이 잘리지 않는지 확인.

## 공통 규칙 (AIEDU 허브)
- 이 앱은 **AIEDU 허브**(https://aiedu-pearl.vercel.app)의 서비스 중 하나다. 허브는 `arenarun23/aiedu` 저장소이고, 빌드 때 이 저장소를 가져와 `/calcul/` 하위 경로로 서비스한다. 허브 전체 구조·점검법은 그 저장소의 `.claude/skills/aiedu-hub`, 서비스별 설명은 `.claude/skills/aiedu-services` 에 있다(필요하면 `arenarun23/aiedu` 를 add_repo 로 붙여 읽는다).
- **정적 파일 + 상대경로만** 사용한다(`/foo.js` 같은 절대경로 금지). 서버·API 키 없음. 빌드가 필요한 구조로 바꾸지 않는다.
- **화면이 깨지지 않고 한 화면에 맞는 것**이 중요하다(주 사용 기기: 삼성 태블릿 가로·폰). 수정 후 1280×560, 1366×650, 800×1280, 390×780, 360×640, 800×360 크기에서 확인한다(허브 저장소 `scripts/fit-check.mjs` 참고). 아이용 큰 터치 버튼은 낮은 화면 미디어쿼리 안에서만 줄인다.
- `main` 에 push 하면 `.github/workflows/notify-hub.yml` 이 허브를 즉시 다시 배포한다(저장소 Secret `HUB_DEPLOY_HOOK` 사용, 20~60초). **Secret 값·훅 주소는 파일/커밋/로그에 쓰지 않는다.**
- push 전 `git fetch origin main` 으로 확인하고, 같은 앱을 다른 세션이 고치고 있었다면 rebase 해서 양쪽 기능을 모두 살린다.
- 반영이 안 보이면 코드보다 먼저 배포본을 비교한다: `curl -s https://aiedu-pearl.vercel.app/apps.json` 의 `calcul` 항목 `sha` 가 이 저장소 최신 커밋과 같은지.
