# CICD

Jest 기반 테스트 프로젝트입니다.

## 프로젝트 구조

```
/
├── .github/workflows/ci.yml   # GitHub Actions CI 설정
├── index.test.js               # Jest 테스트 파일
├── package.json                # 패키지 설정
├── package-lock.json           # 의존성 잠금 파일
├── .gitignore                  # Git 추적 제외 설정
├── doc/
│   ├── github-setup.md         # GitHub 계정 설정 가이드
│   └── ci-workflow-guide.md    # CI 워크플로우 가이드
└── README.md
```

## 시작하기

### 설치

```bash
npm install
```

### 테스트 실행

```bash
npm test
```

---

## CI (GitHub Actions)

`main` 브랜치에 push하거나 PR을 생성하면 GitHub Actions가 자동으로 Jest 테스트를 실행합니다.

- **실행 환경:** ubuntu-latest
- **Node.js 버전:** 18 / 20 / 22 (멀티 버전 테스트)
- **동작 순서:** 코드 체크아웃 → Node.js 설정 → `npm ci` → `npm test`

워크플로우 파일: `.github/workflows/ci.yml`

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20, 22]
    steps:
      - uses: actions/checkout@v4
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'
      - run: npm ci
      - run: npm test
```

---

## Git 관리

### `.gitignore`

```
*
!/.gitignore
!/.github/
!/.github/workflows/
!/.github/workflows/ci.yml
!/index.test.js
!/package.json
!/package-lock.json
```

모든 파일을 기본적으로 무시하고, 필요한 파일만 개별적으로 추적합니다.

### 원격 저장소

```bash
git remote add origin git@github.com-xxxxx:xxxxx/CicdTest.git
git push -u origin main
```

### 계정 전환

여러 GitHub 계정을 사용하는 경우 SSH Host alias로 구분합니다.

```bash
# 현재 전역 사용자 정보 확인
git config --global user.name
git config --global user.email
```

`~/.ssh/config` 설정 예시:

```ini
# 계정 A
Host github.com-userA
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_userA

# 계정 B
Host github.com-userB
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_userB
```

---

## 테스트

```js
test('1 + 1 = 2', () => {
  expect(1 + 1).toBe(2);
});
```
