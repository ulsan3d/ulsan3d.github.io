# 프로젝트 정체

울산대학교 조선해양공학부 안형택 교수 연구실 홈페이지.
호스팅: GitHub Pages (ulsan3d.github.io), 도메인: cfd.ulsan.ac.kr.

# 파일 구조

- `index.html` : 단일 페이지, CSS/JS 인라인
- `assets/`    : 시각화 이미지 (PNG)
- `README.md`, `DEPLOY.md` : 프로젝트 메모

# 디자인 토큰

- 폰트: Fraunces (display, h1/h2), Newsreader (body), JetBrains Mono (mono)
- 컬러:
  - `--paper` `#fcfbf8` (배경)
  - `--ink` `#0e1116` (본문)
  - `--accent` `#a8312f` (강조 라이트 섹션)
  - `--accent-soft` `#d4625f` (강조 다크 섹션 — 다크 배경 위 강조용)
- 미감: Editorial scientific journal — 시각화가 주인공, 텍스트는 보조
- 한국어: 영문 주, 한글은 부 (소형 superscript)

# 작업 원칙

- 보호 클래스 (`.protected`, `.viz-card`) 절대 제거 금지
- 외부 CDN 추가 시 신중 (현재 Google Fonts만)
- 큰 변경 전 반드시 diff 보여주고 확인 받기
- 모든 한글 콘텐츠 UTF-8 유지

# Git 워크플로우

- 단일 `main` 브랜치
- 직접 `main`에 commit/push
- AI 어트리뷰션 표시 절대 금지 (settings.json에서 비활성화됨)
- commit message는 의미 있게: `feat`/`fix`/`style`/`docs`/`refactor` 접두사 권장
