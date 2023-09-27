---
title: TodoList 구현해보기 (2) - React Craco
author: hyebeen
date: 2023-09-22 12:14:00 +0800
categories: [React, TodoList]
tags: [react]
image: /assets/img/posts/craco.png
---

# 📝 React Craco 설정

> [Craco (Create React App Configuration Override)](<(https://craco.js.org/docs/)>)를 사용하여 기본 CRA 설정을 커스터마이징

## 1. Craco란?

- Craco는 Create React App의 설정을 덮어쓰는 방식으로 웹팩 및 개발 서버를 구성할 수 있는 도구

## 2. Craco를 사용한 설정

아래는 Craco를 사용하여 React 프로젝트의 설정을 커스터마이징해 보았습니다.

- 프로젝트 루트 디렉토리에 craco.config.js 파일 생성하고 설정 내용 작성.
  - alias 설정
  - 개발 서버 포트 변경
  - CSS sourcemap 설정을 변경

```js
const path = require("path");

module.exports = {
  webpack: {
    alias: {
      "@": path.resolve(__dirname, "src/"),
      "@components": path.resolve(__dirname, "src/components")
    }
  },
  devServer: {
    port: 3000 // 개발 서버 포트 변경
  },
  css: {
    devSourcemap: true // CSS sourcemap 설정
  }
};
```

## 3. Craco 설정 적용하기

### (1). 프로젝트에 Craco 설치하기

[@craco/craco 설치](https://www.npmjs.com/package/@craco/craco)

```bash
npm install @craco/craco --save

```

### (2). package.json 스크립트 수정

```json
"scripts": {
  "start": "craco start",
  "build": "craco build",
  "test": "craco test",
  "eject": "react-scripts eject"
}

```

## 4. Craco를 사용해 보며

- Craco를 사용하면 React 애플리케이션의 설정을 더욱 세밀하게 제어할 수 있습니다.
- 웹팩 설정, 개발 서버 설정, CSS 관련 설정 등을 Craco를 통해 커스터마이징하여 프로젝트를 보다 효율적으로 개발할 수 있습니다.
