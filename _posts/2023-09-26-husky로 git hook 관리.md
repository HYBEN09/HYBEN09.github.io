---
title: husky로 git hook 관리
author: hyebeen
date: 2023-08-22 16:14:00 +0800
categories: [eslint, husky]
tags: [husky]
---

## 1.  Git Hooks

Git Hooks는 Git 버전 관리 시스템에서 이벤트를 트리거하고 사용자 정의 스크립트를 자동으로 실행하는 기능입니다. 

- 이벤트는 예를 들어 커밋이 생성될 때, 푸시가 발생할 때, 브랜치가 변경될 때와 같은 다양한 경우가 있습니다. 
- Git Hooks은 이러한 이벤트에 대해 자동화 된 작업을 수행 할 수 있으며, 이를 통해 개발 프로세스 자동화, 코드 품질 보증, 프로젝트 유연성 및 안정성 향상 등을 실현할 수 있습니다.
- Git Hooks는 pre-commit, pre-push, post-commit, post-merge, post-checkout 등의 다양한 종류가 있으며, 사용자가 직접 만들어서 적용할 수 있습니다.

```
pre-commit: 커밋하기 전에 실행되며, 코드 품질 검사나 스타일 가이드 준수 등을 확인하는데 사용됩니다.
prepare-commit-msg: 커밋 메시지를 작성하기 전에 실행되며, 커밋 메시지에 특정 정보를 추가하거나 변경할 수 있습니다.
post-commit: 커밋이 완료된 후에 실행되며, 로그 기록이나 통계 정보 갱신 등의 작업에 사용됩니다.
pre-push: 푸시하기 전에 실행되며, 푸시하기 전에 테스트를 실행하거나 코드 검사를 수행하는 등의 작업을 수행할 수 있습니다.
post-receive: 원격 저장소에서 푸시된 후에 실행되며, 자동 배포 또는 특정 작업을 트리거할 때 사용됩니다.
```

## 2.  Husky

### 2-1. 도입배경
- eslint와 prettier를 이용해 프로젝트의 코드 스타일과 품질을 관리하려 하지만, 개발자들이 각자의 작업 환경에서 일관성 있게 사용하지 않으면 효과가 제한적입니다.
- 개발자들이 매번 스스로 검사하고 실행하는 것은 실수의 여지가 있으며, 이를 강제하는 메커니즘이 없습니다.

### 2-2. 문제해결방안
   자동화를 통해 코드 포매팅 및 ESLint 검사를 강제로 적용하는 방안을 구축해야 합니다.

- **자동 포매팅 적용:** pre-commit 에서 Prettier를 실행하여 커밋하기 전에 자동으로 코드 포매팅이 이루어지게 합니다. 이렇게 설정하면, 커밋 시점에 포맷이 일관되게 유지되어 코드 스타일 차이를 최소화할 수 있습니다.

- **ESLint 검사 강제:** pre-push 에서 ESLint를 실행하여, 푸시하기 전에 모든 ESLint 규칙을 만족하는지 검사합니다. 만약 검사에 통과하지 못하면, 푸시 작업이 중단되어 개발자가 수정을 진행하게 됩니다. 이 과정을 통해 푸시된 코드가 무조건 ESLint가 통과된 상태를 유지할 수 있게 됩니다.

### 2-3. 실행방안

(1). Git Hook 도입

- **Git Hook이란?** Git에서 특정 이벤트 발생하기 전,후로 특정 hook 동작을 실행할 수 있게 하는 것 (예: commit, push)입니다.
- Git Hook 설정이 까다롭고, 모든 팀원이 repo를 클론받고 메뉴얼하게 사전 과정을 수행한 후에만 hook이 실행될 수 있습니다.
- 실수로 사전 과정을 시행하지 않으면 hook이 실행되지 않는 문제가 있습니다.

(2). Husky를 이용한 해결법

- Husky는 Git Hooks를 쉽게 관리하도록 도와주며, 메뉴얼한 사전 작업 없이도 팀원들의 작업 환경에서 일관된 Git Hook 실행을 보장해줍니다.
- 이를 통해 팀원 간의 코드 스타일을 일관되게 만들고 품질에 대한 이슈를 최소화할 수 있습니다.

### 2-4. Husky
📚 참고 : [Husky 공식문서](https://typicode.github.io/husky/#/?id=articles) 

- 현대적이고 간편한 Git Hook 설정을 제공하는 npm 패키지
- **장점:** 번거로운 Git Hook 설정을 쉽게 만들며, npm install 과정에서 사전에 세팅된 Git Hook을 모든 팀원에게 적용할 수 있어 팀원 간 일관된 Git Hook 실행이 보장됨
- Husky를 통해 pre-commit 및 pre-push 등의 다양한 Git Hook을 설정할 수 있습니다.

### 2-5. Husky를 통한 Git Hook 적용 방법
(1). 터미널에서 npm install husky --save-dev 명령어로 Husky를 설치

(2). 첫 번째로 Husky를 세팅하는 사람은 npx husky install 명령어를 실행

- npx husky install: Husky에 등록된 hook을 실제 .git에 적용하기 위한 스크립트
- package.json 파일에 postinstall 스크립트를 추가하여 이후 clone 받아 사용하는 팀원들이 npm install 후에 자동으로 husky install이 실행되도록 설정

```bash
// package.json

{
  "scripts": {
    "postinstall": "husky install"
  },
}
```
 

(3). scripts 설정

```bash
// package.json

{
  "scripts": {
    "postinstall": "husky install",
		"format": "prettier --cache --write .",
		"lint": "eslint --cache .",
  },
}
```

(4). pre-commit 및 pre-push hook을 추가합니다

- `npx husky add .husky/pre-commit "npm run format && git add ."` ➡️ pre-commit hook을 추가
- `npx husky add .husky/pre-push "npm run lint"` ➡️ pre-push hook을 추가

- **pre-commit 추가 설명**
  - pre-commit 훅을 lint-staged와 결합하여 사용하면 장점?
    - **포맷팅 후 자동 git add:** lint-staged를 사용하면 커밋 전에 포맷팅을 수행한 뒤, 변경된 파일을 자동으로 Git stage에 올릴 수 있습니다. 이렇게 하면 개발자가 일일이 git add 명령을 실행하지 않아도 됩니다.

    - **변경사항 대상 포맷팅:** lint-staged는 현재 Git stage에 올라온 변경사항 대상으로만 포맷팅을 수행할 수 있습니다
    - 전체 프로젝트의 모든 파일을 검사하는 것이 아니라, 실제로 변경된 부분만 검사하여 효율적으로 코드를 정리할 수 있습니다

요약하면, pre-commit 훅을 lint-staged와 결합하여 사용하면 코드 포맷팅을 자동으로 수행하고, 변경사항에 대한 검사를 효율적으로 처리할 수 있어 코드 품질을 유지하는 데 도움이 됩니다.

📚 참고 : [lint-staged github](https://github.com/okonet/lint-staged#reformatting-the-code)


### 2-6. 참고사항

Git hook에서 eslint 에러 처리에 대한 설정은 프로젝트 및 팀의 정책에 따라 달라질 수 있어서 여러 가지 옵션을 고려하고 결정해야 합니다

 
(1). Error vs. Warn

에러로 설정할 경우, eslint 오류가 발생하면 커밋 또는 푸시가 중지됩니다. 따라서 코드는 오류가 없어야 합니다.
워닝 (warn)으로 설정할 경우, eslint 오류가 발생하더라도 커밋 또는 푸시가 계속됩니다. 그러나 워닝은 경고 메시지로 표시되므로 개발자에게 문제를 알려줍니다.

(2). 경고 여부 고려:

위의 예시에서처럼 no-console 규칙을 warn으로 설정하면 console.log 사용은 허용됩니다. 이것은 팀의 정책에 따라 다를 수 있습니다.
팀이 린트 워닝을 허용하지 않으려면 --max-warnings=0 옵션을 사용하여 eslint 스크립트를 실행하면 됩니다.

```bash
- scripts 설정

// package.json
{
  "scripts": {
    "lint": "eslint --cache --max-warnings=0"
  }
}

```
이렇게 하면 린트가 실패하지 않고 경고 메시지를 포함한 모든 오류를 허용하지 않으며, eslint 경고를 무시하지 않도록 합니다