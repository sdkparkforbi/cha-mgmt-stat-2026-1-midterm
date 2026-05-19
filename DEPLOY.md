# GitHub Pages 게시 가이드

이 폴더의 파일들을 GitHub에 올려 GitHub Pages로 공개하는 방법입니다.
**웹 UI 방식**(빠름, 추천)과 **터미널 방식** 두 가지를 안내합니다.

---

## ⚡ 빠른 체크리스트

1. ✅ 새 저장소 만들기 (예: `cha-mgmt-stat-2026-1-midterm`)
2. ✅ 이 폴더의 **모든 파일**을 업로드 (`.nojekyll` 포함!)
3. ✅ Settings → Pages → Source: **main branch / root** 선택
4. ✅ 1~2분 뒤 표시되는 URL 접속
5. ✅ `index.html`, `exam.html`, `solutions.html`의 `SITE_URL`을 본인 URL로 교체 (OG 메타태그용)

---

## 방법 A. 웹 UI로 업로드 (추천 · 5분)

### 1. 새 저장소 만들기
1. https://github.com/new 접속
2. **Repository name**: `cha-mgmt-stat-2026-1-midterm` (또는 원하는 이름)
3. **Public** 선택 (GitHub Pages 무료 플랜은 Public 필요)
4. **Add a README file** 체크 ❌ (이미 있으니까)
5. **Create repository** 클릭

### 2. 파일 업로드
1. 저장소 메인 화면에서 **Add file → Upload files** 클릭
2. 이 폴더의 **모든 파일을 드래그앤드롭**:
   ```
   index.html, exam.html, solutions.html,
   og-image.png, og-image-preview.png,
   favicon.svg, README.md, DEPLOY.md, .nojekyll
   ```
   > ⚠ `.nojekyll`은 점(.)으로 시작해서 macOS Finder에서 숨김 처리됩니다.
   > Finder에서 `Cmd+Shift+.` 눌러서 숨김 파일 보이게 한 뒤 드래그하세요.
   > Windows 탐색기에선 **보기 → 숨긴 항목** 체크.
3. 하단 commit 메시지: `Initial commit: 경영통계 중간평가 사이트`
4. **Commit changes** 클릭

### 3. GitHub Pages 활성화
1. 저장소 상단 **Settings** 탭 클릭
2. 왼쪽 사이드바 **Pages** 클릭
3. **Source** 섹션:
   - Branch: **`main`**
   - Folder: **`/ (root)`**
   - **Save** 클릭
4. 1~2분 기다린 후 페이지를 새로고침하면 상단에 다음과 같은 메시지 표시:
   > ✓ Your site is live at https://[username].github.io/cha-mgmt-stat-2026-1-midterm/
5. URL을 클릭해서 사이트가 정상 표시되는지 확인

### 4. OG 메타태그 URL 수정 (중요)
카카오톡·Slack 공유 시 썸네일이 보이려면, HTML 파일 안의 OG URL이 실제 사이트 URL과 일치해야 합니다.

1. 저장소에서 `index.html` 클릭 → 우측 상단 ✏ 연필 아이콘 클릭
2. `Ctrl/Cmd + F`로 다음 문자열 검색:
   ```
   sdkparkforbi.github.io/cha-mgmt-stat-2026-1-midterm
   ```
3. 본인 username·저장소명으로 일괄 교체 (예: `myname.github.io/my-repo`)
4. **Commit changes**
5. `exam.html`, `solutions.html`에도 동일하게 적용

> 💡 한 번에 처리하려면 아래 **방법 B (터미널)** 의 sed 명령으로 일괄 변경하면 편합니다.

---

## 방법 B. 터미널로 업로드 (Git CLI)

이 폴더에서 그대로 실행:

```bash
# 0. (선택) OG URL을 본인 저장소 주소로 일괄 변경
#    YOUR_USERNAME, YOUR_REPO를 본인 값으로 교체
OLD="sdkparkforbi.github.io/cha-mgmt-stat-2026-1-midterm"
NEW="YOUR_USERNAME.github.io/YOUR_REPO"
# macOS의 BSD sed
sed -i '' "s|$OLD|$NEW|g" index.html exam.html solutions.html README.md
# Linux의 GNU sed면 (-i '' → -i)
# sed -i "s|$OLD|$NEW|g" index.html exam.html solutions.html README.md

# 1. Git 초기화
git init
git add .
git commit -m "Initial commit: 경영통계 중간평가 사이트"

# 2. GitHub에서 빈 저장소를 먼저 만들어두고 (https://github.com/new), URL 복사
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main

# 3. GitHub Pages 활성화 (gh CLI 설치되어 있으면)
gh repo edit --enable-pages
# 또는 웹에서 Settings → Pages → main / root → Save
```

---

## 🔍 게시 후 확인 사항

### 1. 사이트 표시 확인
- `https://[username].github.io/[repo]/` 접속
- 랜딩 페이지에서 두 카드 클릭 → 문제지·해설지가 잘 열리는지
- 모바일에서도 확인 (반응형 동작)
- **수식 렌더링**: `\( \mu \)` 등이 깔끔한 수학 글꼴로 변환되는지

### 2. OG 썸네일 확인
- **카카오톡 디버거**: https://developers.kakao.com/tool/debugger/sharing
  - URL 입력 → "디버그" → 캐시 강제 갱신
- **Facebook Sharing Debugger**: https://developers.facebook.com/tools/debug/
- **Twitter Card Validator**: https://cards-dev.twitter.com/validator

> 카카오톡은 한 번 캐시되면 며칠씩 안 바뀌는 경우가 많습니다. 디버거에서 "Fetch new scrape" 한 번 돌려주세요.

### 3. 흔한 트러블슈팅

| 증상 | 원인 / 해결 |
| --- | --- |
| 404 페이지 | Pages 활성화 후 빌드까지 1~2분 소요. 좀 기다리고 새로고침. |
| 한글 폰트가 깨짐 | 인터넷 차단 환경. 외부 폰트(Google Fonts) 로드 실패. 학교 와이파이 등이면 무관. |
| 수식이 `\( \mu \)` 그대로 보임 | MathJax CDN 차단. 잠시 후 다시 시도하거나 다른 네트워크에서 확인. |
| 카톡 썸네일이 안 뜸 | 카카오톡 디버거에서 "Fetch new scrape" 실행. OG URL이 정확한지 재확인. |
| CSS·이미지가 안 보임 | `.nojekyll` 파일이 누락된 경우 발생. 저장소에 다시 업로드. |

---

## 🔁 자료 업데이트하기

문제지·해설지를 나중에 수정하려면:

### 웹 UI
1. 저장소에서 해당 HTML 파일 클릭 → ✏ 편집 → Commit changes
2. 1~2분 뒤 사이트 자동 반영

### 터미널
```bash
# 파일 수정 후
git add .
git commit -m "Update: 8번 문제 보기 수정"
git push
```

---

## 🔒 (선택) 비공개로 운영하고 싶다면

GitHub Pages 무료 플랜은 Public 저장소만 지원하므로:

- **GitHub Pro/Team 플랜** 사용 시 Private 저장소도 Pages 가능
- **간단한 접근 제한**이 필요하면:
  - LMS에만 링크 게시 + URL을 추측하기 어렵게 작성 (`-h7f3b9` 같은 무작위 접미사)
  - 또는 LMS의 인증된 페이지에 HTML 직접 임베드 (외부 GitHub 사용 안 함)

---

© 2026 박대근 · CHA University
