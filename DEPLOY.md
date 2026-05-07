# GitHub Pages 배포 가이드 — cfd.ulsan.ac.kr

교수님 환경(Windows + WSL, GitHub `@ulsan3d`)에 맞춘 7단계 가이드.

---

## 전체 흐름 한눈에

```
[1] repo 이름 결정              ────┐
[2] GitHub repo 생성                │  로컬 작업
[3] WSL에서 git push                │  (며칠~몇주 가능)
[4] GitHub Pages 활성화         ────┤
[5] 임시 URL로 충분히 검토      ────┘
                                    │
[6] 학교 OIT에 CNAME 변경 요청 ────┐  도메인 전환
[7] HTTPS 활성화                ────┘  (1회 작업)
```

핵심 원칙: **[6]은 [5]에서 완전히 만족할 때만**. 그 전까지 cfd.ulsan.ac.kr은 기존 Google Sites 그대로 살아 있음.

---

## [1] repo 이름 결정 — 두 옵션

| 옵션 | repo 이름 | 임시 URL | 의미 |
|---|---|---|---|
| **A** | `ulsan3d.github.io` | `https://ulsan3d.github.io/` | 사용자명과 동일 — GitHub Pages 특수 repo, root 경로 |
| **B** | `cfd-lab` (또는 자유) | `https://ulsan3d.github.io/cfd-lab/` | 일반 repo, 하위 경로 |

**권장: 옵션 A.** 이유:
- root URL에 사이트가 직접 뜸 (path 없음)
- custom domain(cfd.ulsan.ac.kr) 연결 시에도 가장 단순
- 향후 `ulsan3d.github.io`를 다른 용도로 쓸 계획이 없다면 가장 깔끔

**단점**: GitHub 계정당 옵션 A repo는 1개만 가능. 향후 다른 프로젝트 페이지가 필요하면 그건 옵션 B 형식으로 별도 만들면 됨.

이하 가이드는 **옵션 A 기준**으로 작성. 옵션 B를 고르시면 `ulsan3d.github.io` → 원하는 이름으로만 바꾸시면 됨.

---

## [2] GitHub repo 생성 — 웹 GUI

1. github.com 로그인 → 우상단 `+` → `New repository`
2. **Repository name**: `ulsan3d.github.io`
3. **Description**: `Computational Ship Hydrodynamics Lab — University of Ulsan`
4. **Public** 선택 (GitHub Pages 무료 플랜에서는 Public 필수)
5. README, .gitignore, license 모두 **체크하지 않음** (로컬에서 push할 예정)
6. `Create repository` 클릭

생성 후 `https://github.com/ulsan3d/ulsan3d.github.io` 접속됨.

---

## [3] WSL에서 git push

WSL 터미널 열고 (어떤 디스트로든 무관):

### 3-1. 데모 폴더로 이동
zip을 풀어둔 위치로. 예시:
```bash
cd /mnt/c/Users/<Windows사용자명>/Downloads/cfd-site
# 또는 WSL 홈에 복사해두셨다면:
cd ~/cfd-site
```

`ls` 했을 때 `index.html`, `assets/`, `README.md`가 보여야 함.

### 3-2. git 초기 설정 (한 번만)
```bash
git config --global user.name "Hyung Taek Ahn"
git config --global user.email "<교수님 GitHub 등록 이메일>"
```

이미 다른 작업으로 설정해 두셨으면 생략.

### 3-3. SSH 키 (선택, 권장) 또는 토큰 인증
GitHub은 비밀번호 push를 막아놓아서 두 가지 중 하나가 필요:

**방법 A — SSH 키 (한 번 셋업하면 영구):**
```bash
ssh-keygen -t ed25519 -C "<교수님 이메일>"
# 엔터 3번 (기본 경로, 빈 passphrase)
cat ~/.ssh/id_ed25519.pub
```
출력된 공개키 전체를 복사 → GitHub `Settings → SSH and GPG keys → New SSH key` → 붙여넣기

**방법 B — Personal Access Token:**
GitHub `Settings → Developer settings → Personal access tokens → Tokens (classic)` → Generate → `repo` 권한 체크. 발급된 토큰을 push 시 비밀번호 자리에 입력. 토큰은 1번만 표시되므로 안전한 곳에 저장.

방법 A가 장기적으로 편함.

### 3-4. push
```bash
git init -b main
git add .
git commit -m "Initial site"
git remote add origin git@github.com:ulsan3d/ulsan3d.github.io.git
# (방법 B를 쓰면 위 줄을 https://github.com/ulsan3d/ulsan3d.github.io.git 로)
git push -u origin main
```

push 성공하면 GitHub repo 페이지에서 파일이 보임.

---

## [4] GitHub Pages 활성화

1. repo 페이지 → `Settings` 탭 → 좌측 메뉴 `Pages`
2. **Source**: `Deploy from a branch`
3. **Branch**: `main` / `/ (root)` 선택 → `Save`
4. 1~2분 대기

상단에 다음과 같이 뜸:
```
✓ Your site is live at https://ulsan3d.github.io/
```

이 URL을 브라우저로 열면 **로컬에서 보던 것과 동일한 사이트**가 인터넷에서 보여짐. 도메인 cfd.ulsan.ac.kr은 아직 전혀 건드리지 않은 상태.

---

## [5] 임시 URL로 충분히 검토

이 단계가 가장 중요함. **시간 제한 없이** 며칠 또는 몇 주 동안:

- 다양한 디바이스에서 접속 (Windows/Mac/모바일)
- 다양한 브라우저(크롬, 사파리, 엣지)에서 점검
- 동료·학생에게 임시 URL 공유해서 피드백 수집
- 콘텐츠 보강(논문, 이미지, 학생 모집 등)
- 디자인 미세 조정

수정할 때마다:
```bash
# WSL에서
git add .
git commit -m "Update XXX section"
git push
```
push 후 1분 내로 임시 URL에 자동 반영됨.

**모든 게 만족스러울 때만 [6]으로**.

---

## [6] 학교 OIT에 CNAME 변경 요청

### 6-1. 요청 내용 (메일 템플릿)

> 정보화팀 담당자님,
>
> 안녕하십니까. 조선해양공학부 안형택 교수입니다.
>
> 현재 운영 중인 연구실 홈페이지 도메인 **cfd.ulsan.ac.kr** 의 DNS 설정 변경을 요청드립니다.
>
> 현재: CNAME `cfd.ulsan.ac.kr` → `ghs.googlehosted.com` (Google Sites)
> 변경: CNAME `cfd.ulsan.ac.kr` → `ulsan3d.github.io` (GitHub Pages)
>
> 호스팅 플랫폼을 Google Sites에서 GitHub Pages로 이전합니다. 도메인은 동일하게 유지됩니다.
>
> 변경 후 약 1~24시간(DNS TTL) 내에 전환이 완료되며, 그 사이 일부 사용자에게는 이전 사이트가 보일 수 있습니다.
>
> 감사합니다.
> 안형택 드림

### 6-2. GitHub 측 설정 (CNAME 요청 직후 진행)

1. repo `Settings → Pages → Custom domain` 입력란에 `cfd.ulsan.ac.kr` 입력 → `Save`
2. 자동으로 repo 루트에 `CNAME` 파일이 생성됨 (이게 GitHub에 도메인을 알려주는 역할)
3. DNS 검증이 진행됨 (수 분~수 시간)

### 6-3. DNS 전파 확인

WSL 터미널에서:
```bash
dig cfd.ulsan.ac.kr +short
# 또는 dig가 없으면
host cfd.ulsan.ac.kr
```

`ulsan3d.github.io` 또는 GitHub Pages IP(185.199.108.153 등)가 떠야 함. 이전 값(`ghs.googlehosted.com`)이 보이면 아직 학교 OIT 측 변경이 반영 안 된 것 — 더 기다리거나 OIT 재확인.

---

## [7] HTTPS 활성화

DNS 전파가 완료된 후 (보통 [6] 후 1~24시간):

1. repo `Settings → Pages` 페이지에 `Enforce HTTPS` 체크박스가 활성화됨 (회색→파란색)
2. 체크
3. GitHub이 Let's Encrypt SSL 인증서를 자동 발급·갱신함

이걸로 `https://cfd.ulsan.ac.kr` 접속 시 자물쇠 표시됨. 끝.

---

## 운영 모드 — 일상 업데이트

배포 후의 콘텐츠 수정은 모두 동일한 패턴:

```bash
cd ~/cfd-site
# 파일 수정 (index.html, 이미지 추가 등)
git add .
git commit -m "Add new publication"
git push
```

**1~2분 내로 cfd.ulsan.ac.kr에 자동 반영됨.** GitHub Actions가 자동으로 빌드·배포함 (정적 사이트라 빌드도 거의 즉시).

---

## 비상 상황 — 롤백

만약 push 후 문제가 생기면:

```bash
# 직전 커밋으로 되돌리기
git reset --hard HEAD~1
git push --force
```

또는 학교 OIT에 CNAME을 다시 `ghs.googlehosted.com`로 되돌리도록 요청. Google Sites 원본은 **삭제하지 않는 한 살아 있음**. 즉 CNAME만 되돌리면 즉시 복구.

---

## 흔한 트러블슈팅

| 증상 | 원인 | 해결 |
|---|---|---|
| `git push` 시 비밀번호 거부 | GitHub은 패스워드 push 차단 | SSH 키 또는 PAT 토큰 사용 |
| Pages URL이 404 | 빌드 미완료 | 1~2분 대기, repo의 Actions 탭에서 빌드 상태 확인 |
| Custom domain 검증 실패 | DNS 전파 미완료 | 24시간 대기 후 재시도 |
| HTTPS 옵션 회색 | DNS 검증 미완료 | DNS 전파 확인 후 다시 시도 |
| 한글 파일명 깨짐 | git core.quotepath 설정 | `git config --global core.quotepath false` |

---

## 다음 단계 — 운영 향상 (선택)

기본 배포 후 여유될 때 추가 가능:

1. **404 페이지 커스터마이즈**: `404.html` 파일 추가
2. **사이트맵**: `sitemap.xml` 생성으로 검색엔진 노출 개선
3. **Google Search Console 등록**: cfd.ulsan.ac.kr 검색 색인 관리
4. **워터마크 자동화 스크립트**: Pillow로 신규 이미지에 영구 워터마크 합성
5. **GitHub Actions로 이미지 자동 압축**: push 시 PNG/JPG 자동 최적화

이 중 필요한 것 있으면 별도 가이드 작성 가능.
