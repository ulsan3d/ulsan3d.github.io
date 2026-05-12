# cfd.ulsan.ac.kr — 데모 프로토타입

> 울산대학교 전산선박유체공학 연구실(안형택 교수)의 단일 페이지 정적 사이트 프로토타입 — ULSAN3D 시각화 갤러리, 경력, 솔버 소개, 논문 목록을 한 페이지에 담음.

## 무엇인가

현재 Google Sites 기반 홈페이지(cfd.ulsan.ac.kr)의 자체 호스팅 정적 사이트 데모임. 단일 HTML 페이지에 PPTX(2026_05_오픈랩_포스터)에서 추출한 시각화 이미지를 적용했고, 이미지 보호 1·2단계가 실제 동작함.

## 폴더 구조

```
cfd-site/
├── index.html         ← 사이트 본체 (모든 CSS/JS 인라인)
├── assets/            ← 시각화 이미지
│   ├── hero_golfball_wake.png  (← image3)
│   ├── kcs_wake_rgb.png        (← image10)
│   ├── kcs_vortex_side.png     (← image11)
│   ├── free_surface_kcs.png    (← image1)
│   ├── auv_waves.png           (← image9)
│   ├── ship_wake.png           (← image5, 미사용 예비)
│   ├── profile.jpg             (← image7, 미사용 예비)
│   └── ulsan_logo.gif          (← image2, 미사용 예비)
└── README.md
```

## 보는 방법 (3가지)

### 가장 간단 — 더블클릭
`index.html`을 더블클릭하면 기본 브라우저(크롬 권장)에서 바로 열림. 단일 페이지라 file:// 프로토콜로도 100% 동작함.

### WSL에서 서버 띄우기 (실제 배포와 동일한 환경)
WSL 터미널에서:
```bash
cd /mnt/c/Users/<사용자>/cfd-site
python3 -m http.server 8000
```
Windows 크롬에서 `http://localhost:8000` 접속. WSL2는 localhost가 자동 공유되므로 추가 설정 불필요.

### VSCode Live Server
VSCode에서 폴더 열고 → 우클릭 `Open with Live Server` → 저장 즉시 자동 새로고침.

## 적용된 이미지 보호

| 단계 | 방식 | 동작 |
|---|---|---|
| 1 | 우클릭 차단 (JS) | 이미지·시각화 카드에서 우클릭 메뉴 비활성 |
| 2 | 드래그 차단 (JS) | 이미지를 데스크톱·다른 탭으로 끌어내릴 수 없음 |
| 3 | Ctrl+S, Ctrl+U, Ctrl+P 차단 (JS) | 페이지 저장·소스 보기·인쇄 단축키 무력화 |
| 4 | CSS background-image 트릭 | `<img>` 태그를 안 써서 우클릭 시 "이미지 저장" 메뉴 자체가 안 뜸 |
| 5 | user-select / user-drag CSS | 텍스트·이미지 선택 차단 |
| 6 | CSS 워터마크 오버레이 (시뮬레이션) | 데모용 — 실제 배포 시에는 Pillow로 이미지에 영구 합성 예정 |

**테스트 방법**: 페이지를 띄운 뒤 골프공 wake 시각화 위에서 우클릭 시도 → 메뉴 안 뜸. 이미지를 데스크톱으로 끌어보기 → 안 됨. Ctrl+S 시도 → 무력화됨.

**한계**: F12 DevTools를 열어 CSS의 `url()`을 추출하면 우회 가능. 이는 모든 클라이언트측 보호의 본질적 한계이며, 일반 사용자(80–90%) 차단을 목표로 함. 진짜 보호는 워터마크 합성(3단계 풀 적용)이 담당.

## 디자인 방향

- **Editorial scientific journal** 미감 — 학술 출판물 분위기 + 강력한 시각화
- **타이포그래피**: Fraunces (display, variable serif) + Newsreader (body) + JetBrains Mono (mono accent) — 모두 Google Fonts에서 자동 로드
- **컬러**: Off-white paper(`#fcfbf8`) + 깊은 잉크(`#0e1116`) + 학술지 빨강 액센트(`#a8312f`)
- **레이아웃**: Asymmetric grid, 큰 hero 타이포, 시각화 갤러리, 다크 톤 ULSAN3D 솔버 섹션, editorial-style 논문 리스트

## 다음 단계 — 논의 필요 사항

데모를 보신 후 결정/지시하실 항목:

1. **콘텐츠**
   - 누락 섹션 추가 (특허, Honors, 졸업생 등을 살릴지?)
   - Publications 전체 30+편 리스트 — 별도 페이지로 분리할지, 아래로 이어붙일지
   - About 섹션 — 짧은 narrative 추가 여부

2. **디자인**
   - 컬러 톤 (현재 학술지 따뜻한 톤 — 더 차갑게 / 더 어둡게 등 변경 의향)
   - 한국어 비중 (현재 영문 주, 한글 부 — 비중 조정?)
   - 워터마크 문구 (현재 `ULSAN3D · H.T.AHN` — 다른 표현 선호 시)

3. **구조**
   - 멀티 페이지로 분할할지 (Research / Publications / Lab / Contact 등)
   - 학생 모집·연구실 소식 등 동적 콘텐츠 영역 추가 여부

4. **호스팅**
   - GitHub Pages 진행 여부
   - repo 명명 (예: `ulsan3d/cfd-lab`, `ulsan3d/ulsan3d.github.io`)

## 기술 메모

- 외부 의존성: Google Fonts (Fraunces / Newsreader / JetBrains Mono)
  - 인터넷 없는 환경에서는 시스템 폴백(Georgia, 맑은 고딕)으로 동작
  - 완전 오프라인이 필요하면 폰트를 로컬에 내장 가능 (woff2 다운로드 후 @font-face)
- 빌드 도구 없음 — 순수 HTML/CSS/JS, 정적 파일 그대로
- 반응형 — 900px 이하에서 모바일 레이아웃 자동 적용

