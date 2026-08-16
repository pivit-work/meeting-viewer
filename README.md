# meeting-viewer
PIVIT 회의록 &amp; 기획 문서 뷰어

- 회의록: <https://pivit-work.github.io/meeting-viewer/>
- 기획서: <https://pivit-work.github.io/meeting-viewer/specs.html>

## 기획서 뷰어를 처음 열 때 (토큰 발급)

기획서는 private 레포 `pivit-work/pivit-specs` 에서 직접 읽어온다.
GitHub Pages 는 정적 호스팅이라 서버가 대신 인증해 줄 수 없어서,
**보는 사람마다 본인 계정의 읽기 토큰을 한 번 넣어야 한다.**
토큰은 해당 브라우저의 localStorage 에만 저장되고 GitHub 외 어디로도 전송되지 않는다.

1. [토큰 발급 페이지 열기](https://github.com/settings/tokens/new?scopes=repo&description=Pivit+Viewer)
   (Settings → Developer settings → Personal access tokens → Tokens (classic))
2. `repo` 스코프가 체크돼 있는지 확인한다. Expiration 은 90 days 이상 권장.
3. **Generate token** → `ghp_…` 문자열 복사
4. 뷰어의 잠금창에 붙여넣고 **저장하고 시작하기**

만료되면 잠금창이 다시 뜬다. 같은 절차로 새 토큰을 넣으면 된다.
우측 상단 **토큰 재설정** 으로 언제든 지울 수 있다.

### 안 열릴 때

| 화면 | 원인 | 조치 |
|------|------|------|
| `🔐 GitHub 토큰 필요` 잠금창 | 토큰 미입력 또는 만료 | 위 절차로 발급해 입력 |
| `토큰이 만료됐거나 잘못됐습니다` | 토큰 오타·폐기·만료 | 새로 발급 |
| `접근 거부(403)` | `repo` 스코프 누락, 또는 요청 일시 제한 | 스코프 확인 후 재발급 / 잠시 후 재시도 |
| `찾을 수 없음(404)` | 그 계정에 `pivit-specs` 접근 권한이 없음 | 레포 관리자에게 초대 요청 |
| 목록은 뜨는데 미리보기가 백지 | 옛 버전이 브라우저에 캐시됨 | 강력 새로고침 (⌘⇧R / Ctrl+Shift+R) |

## 구조

정적 파일 2개가 전부다. 빌드 단계 없이 main 브랜치가 그대로 Pages 로 배포된다.

| 파일 | 내용 |
|------|------|
| `index.html` | 회의록 뷰어 — 결정사항 검색, 아키텍처 탭, AI 리서치 |
| `specs.html` | 기획서 뷰어 — pivit-specs 의 JSX·MD·HTML 을 트리로 열람·렌더 |

`specs.html` 의 JSX 미리보기는 화면 기획이 쓰는 전역 디자인 토큰(`window.PIVIT`)을
pivit-specs 의 SSOT `scripts/pivit_tokens.py` 에서 매번 읽어 주입한다.
토큰 값을 이 레포로 복사해 오지 않는다 — 복사본은 반드시 드리프트한다.
