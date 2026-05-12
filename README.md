# hwpxl 브라우저 단독 실행판

`index.html` 한 페이지로 HWP / HWPX / ODT 문서를 **브라우저 안에서** 미리 보고
Excel(.xlsx)로 변환합니다. **서버도 Python도 설치할 필요 없습니다.**

- 파일이 **서버로 전송되지 않습니다** — 브라우저 메모리 안에서만 처리됩니다.
- macOS · Windows · Linux 어디서나 동일하게 동작합니다.

## 폴더 안에 무엇이 있나요

```
web-app/
├── index.html                       # 메인 페이지 (이 파일을 열면 시작)
├── hwpxl-0.1.0-py3-none-any.whl     # 변환 엔진 (Python 패키지)
└── README.md                        # 이 문서
```

## 어떻게 실행하나요

### 방법 1: 로컬에서 (개발자/PC에 익숙한 사람)

브라우저 보안 정책상 `index.html`을 더블클릭(`file://`)으로 열면 같은 폴더의
`.whl` 을 못 가져옵니다. **로컬 HTTP 서버**를 한 번 띄워야 합니다.

```bash
cd web-app
python3 -m http.server 8123
# 브라우저에서 http://127.0.0.1:8123 접속
```

### 방법 2: 정적 호스팅에 올리기 (가장 편함, 누구나)

이 폴더를 통째로 **GitHub Pages / Netlify / Vercel / Cloudflare Pages**
같은 정적 호스팅에 올리면 됩니다. 그러면 URL만 공유해서 누구나
브라우저로 즉시 접속·사용 가능합니다.

**GitHub Pages 예시 (가장 간단):**

1. GitHub에 새 리포 생성, 이 `web-app/` 폴더 내용물을 푸시.
2. Settings → Pages → Source: `main` 브랜치 / root.
3. 1~2분 후 `https://<유저명>.github.io/<리포명>/` 에서 접근.

### 방법 3: 받는 사람도 같은 방식으로

이 폴더 전체를 zip으로 압축해 전달하면, 받는 사람도
`python3 -m http.server` 한 줄로 즉시 사용할 수 있습니다.

## 처음 로드 시 무엇이 일어나나요

1. Pyodide(웹어셈블리 Python 런타임)를 CDN에서 한 번 다운로드 (~6MB).
2. `lxml` (Pyodide 빌트인) + `olefile`, `openpyxl` (PyPI에서 자동) 설치.
3. 같은 폴더의 `hwpxl-*.whl` 설치.

이후엔 브라우저 캐시 덕에 즉시 시작됩니다.

## 변환 흐름

- **본문 미리보기**: HWP → 블록(문단·표) 추출 → HTML 렌더 → iframe에 표시.
- **Excel 다운로드**: HWP → 블록 추출 → openpyxl로 .xlsx 작성 → 다운로드 트리거.
- 셀 병합·헤딩·문서 흐름이 그대로 보존됩니다.

## 한계

- 첫 로딩 시 Pyodide 5~10초.
- 매우 큰 HWP(50MB 이상)는 브라우저 메모리 제한에 걸릴 수 있습니다.
- 이미지·도형·수식은 추출되지 않습니다 (본문 + 표만).
- 암호화된 문서는 처리 불가.

## 라이선스

MIT. 자세한 것은 부모 디렉토리 `LICENSE` 참고.
