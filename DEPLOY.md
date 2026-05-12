# GitHub Pages로 배포하기

이 폴더(`web-app/`) 안의 내용을 GitHub에 push하고 Pages를 켜면,
누구나 URL 하나만 클릭해서 사용할 수 있는 웹 앱이 됩니다.

## 0. 준비

- GitHub 계정
- 컴퓨터에 `git` 설치 (`git --version` 으로 확인)

## 1. GitHub에서 새 리포 생성

브라우저로 https://github.com/new 접속:

- **Repository name**: 원하는 이름 (예: `hwpxl-web`)
- **Public** 선택 (Pages를 무료로 쓰려면 public이어야 합니다)
- "Add a README", ".gitignore", "license" 는 **모두 체크하지 마세요** (충돌 방지)
- **Create repository** 클릭

생성 직후 화면에 보이는 `git remote add origin ...` URL을 복사해둡니다.

## 2. 이 폴더를 push

터미널에서 `web-app/` 폴더 안으로 이동 후, 아래 명령을 순서대로 실행:

```bash
cd web-app

git init
git add -A
git commit -m "Initial release: hwpxl web app"
git branch -M main

# ↓ 1번에서 복사한 URL로 교체
git remote add origin https://github.com/<유저명>/hwpxl-web.git

git push -u origin main
```

> ⚠️ 처음 push 시 GitHub 비밀번호 대신 **Personal Access Token**을 요구합니다.
> https://github.com/settings/tokens/new 에서 토큰 생성(`repo` 권한 체크) →
> push 프롬프트의 Password 자리에 붙여넣기.

## 3. Pages 활성화

방금 만든 리포의 GitHub 페이지에서:

1. **Settings** 탭 → 좌측 **Pages** 메뉴
2. **Build and deployment** 섹션
   - Source: **Deploy from a branch**
   - Branch: **main** / **/ (root)**
3. **Save** 클릭

1~2분 후 같은 페이지 상단에 `Your site is live at https://<유저명>.github.io/hwpxl-web/`
링크가 표시됩니다.

## 4. 끝 — URL을 누구에게나 공유하세요

받는 사람은 그 URL만 클릭하면:

- 첫 방문 5~10초 대기 (Pyodide 다운로드)
- 그 후 .hwp / .hwpx / .odt 파일을 드래그앤드롭 → 즉시 미리보기 또는 .xlsx 다운로드
- 파일은 받는 사람 브라우저 안에서만 처리됩니다 (어떤 서버로도 전송 X)

## 변경 사항 푸시

`index.html` 이나 wheel을 업데이트하면:

```bash
git add -A
git commit -m "Update"
git push
```

→ 자동으로 Pages도 갱신됩니다 (보통 1~2분 이내).

## 자주 묻는 것

**Q. 폴더(서브디렉토리)에 두고 싶어요.**
A. `git mv` 로 옮긴 뒤, GitHub URL은 `https://<유저명>.github.io/<리포명>/<경로>/` 가 됩니다.

**Q. 커스텀 도메인 쓰고 싶어요.**
A. Settings → Pages → Custom domain 에 도메인 입력 후 DNS의 CNAME을 `<유저명>.github.io` 로 설정.

**Q. 비공개로 쓰고 싶어요.**
A. GitHub Pro/Team/Enterprise 플랜의 Private Pages가 필요합니다. 무료 계정은 public 리포만 Pages 가능.
