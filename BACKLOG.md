# kimjungeun 백로그

사이트에 공개되지 않는다(`_config.yml` exclude). 요한님 결정이 필요한 것은 `ai-assistant/TODO.md` 에도 행이 있다.

## 2026-11-06 위클리 리뷰에서 찾고 이번에 하지 않은 것

판정 요약(같은 날 08:40~09:10 KST 실측): 사이트는 쓰이고 있다(라이브 `/`·`404.html` 이 repo 파일과 바이트 동일,
Pages `built`·`https_enforced`, 인증서 만료 2027-01-10). gitleaks 로 이력·작업 트리·공개 Actions 로그를 검사했고 비밀은 0건이었다(양성 대조군에서는 잡혔다).
repo secrets 0. 문서 어긋남 2건(pullfrog 설명, www 서술 없음)은 CLAUDE.md 에서 고쳤다.

### 🙋 요한님 결정 — 설정·DNS 변경이라 위임 범위 밖

| # | 무엇 | 근거 | 권장 |
|---|---|---|---|
| D1 | pullfrog 워크플로 처분 | `disabled_manually`(2026-09-12). 마지막 3회(09-11)는 `billing exhausted` 로 실패. secrets 0. public repo 에서 서드파티 `pullfrog/pullfrog@v0` 를 태그로만 고정하고 `id-token` 쓰기 권한을 준다. `ai-assistant` 의 `runner-fleet.sh onboard` 가 일괄 활성화할 수 있다 | 비활성을 유지하고 일괄 활성화 대상에서 kimjungeun 을 뺀다. 파일 제거는 그다음 선택지 |
| D2 | secret scanning · push protection 켜기 | `security_and_analysis` 5항목 모두 disabled. dependabot alerts 403 "disabled", secret-scanning 404 "disabled" | 켠다. public repo 에서는 무료이고 표준이다 |
| D3 | GitHub Pages 도메인 검증(takeover 방지) | Pages API `protected_domain_state=null`. Pages 를 끄거나 repo 를 지웠는데 A 레코드 4개가 남으면 남이 이 도메인을 붙일 수 있다 | GitHub 계정 Pages 설정에서 `kimjungeun.com` 을 검증한다(Cloudflare TXT 1개) |
| D4 | `www` 레코드 | `www.kimjungeun.com` NXDOMAIN. 그 주소로 들어오는 방문자는 DNS 오류를 본다 | Cloudflare 에 `www CNAME yohangwak.github.io`(DNS only)를 추가한다. 도메인 사용자가 바뀔 예정이면 보류 |

### 기록만 — 지금 할 이유가 약함

- B1. 보안 헤더 0개. GitHub Pages 는 응답 헤더를 바꿀 수 없고, Cloudflare 는 DNS only 다. `<meta http-equiv="Content-Security-Policy" content="default-src 'none'; style-src 'unsafe-inline'; img-src data:">` 를 넣으면 「외부 요청 0」을 브라우저가 강제한다. 스크립트·폼이 없는 페이지라 이득이 작다.
- B2. 링크 미리보기 이미지(`og:image`) 없음(`twitter:card summary`). 이미지 자산이 생기면 넣는다.
- B3. 404 응답에는 `cache-control` 헤더가 없다. GitHub Pages 동작이라 제어할 수 없다.
- B4. 초기 3커밋의 작성자 이메일이 개인 주소다. 2026-09-18 점검에서 「그대로 두기」로 정했다. 이력을 다시 쓰려면 force push 가 필요하다.
