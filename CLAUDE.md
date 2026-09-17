# kimjungeun

`kimjungeun.com` 의 「홈페이지 준비 중」 안내 페이지. 빌드 도구·의존성·테스트 없는 정적 HTML 페이지(`index.html`·`404.html`)다.

## 구조

| 파일 | 역할 |
|---|---|
| `index.html` | 안내 페이지. CSS 는 인라인, 파비콘은 data URI — 외부 요청 0. 링크 미리보기용 Open Graph·canonical 메타 포함 |
| `404.html` | 없는 주소에 GitHub Pages 가 404 상태로 내주는 페이지. `index.html` 스타일을 **복사**해 둔다(한쪽을 바꾸면 같이) |
| `CNAME` | GitHub Pages 커스텀 도메인(`kimjungeun.com`). **지우면 도메인이 풀린다** |
| `_config.yml` | GitHub Pages(Jekyll) 빌드 설정. 사이트에 올리지 않을 파일 목록 |
| `.gitignore` | `*.png` — 이미지 자산을 커밋하려면 `git add -f` |
| `.github/workflows/pullfrog.yml` | 수동 실행 AI 에이전트 워크플로. 손대지 않는다 |

## 배포 — `main` push = 라이브

GitHub Pages 가 `main` 브랜치 루트를 **Jekyll 로 빌드**해 `https://kimjungeun.com/` 에 올린다
(별도 CI·배포 스크립트 없음). 머지하면 몇 분 안에 반영되고, 응답이 `Cache-Control: max-age=600`
이라 브라우저·CDN 에 최대 10분 옛 페이지가 남을 수 있다.

⚠️ **Jekyll 이 돌기 때문에 루트에 둔 `.md` 파일도 페이지로 변환돼 공개된다**
(GitHub Pages 는 front matter 없는 마크다운도 변환한다). 사이트에 나가면 안 되는 문서를 새로
추가하면 `_config.yml` 의 `exclude` 에 같이 넣는다. `_` 나 `.` 로 시작하는 경로는 원래 제외된다.

## 확인 방법

```sh
# 로컬 미리보기 (저장소 루트에서)
python3 -m http.server 8000        # → http://localhost:8000/

# 모바일·데스크톱 캡처 (헤드리스 Chrome 직접 호출 금지 — shot 래퍼 사용)
shot http://localhost:8000/ "$TMPDIR/kje-375.png" 375
shot http://localhost:8000/ "$TMPDIR/kje-1440.png" 1440

# 머지 후 라이브 확인
curl -sS -o /dev/null -w '%{http_code}\n' https://kimjungeun.com/            # 200 이어야 한다
curl -sS -o /dev/null -w '%{http_code}\n' https://kimjungeun.com/CLAUDE.html # 404 이어야 한다(문서 비공개)
```

## 주의

- 이메일 주소·소개 문구는 여러 곳에 중복돼 있다 — `index.html` 본문·`<meta name="description">`·`og:description`, 그리고 `404.html` 본문. 바꿀 땐 모두.
- `env(safe-area-inset-*)` 에는 대체값을 반드시 준다 — 없으면 미지원 환경에서 선언 전체가 무효가 된다
  (`index.html` 주석 참고).
- 외부 폰트·스크립트·분석 태그를 넣으면 「외부 요청 0」이 깨진다. 넣을 땐 의도적으로.
