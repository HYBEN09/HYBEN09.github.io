---
title: React Query
author: hyebeen
date: 2023-10-15 18:48:00 +0800
categories: [React, React-Query]
tags: [react-query]
image: https://tech.kakao.com/storage/2022/06/01.png
---

## 1. React Query란?

[@공식사이트](https://tanstack.com/query/v3/)

React Query는 데이터 Fetching, 캐싱, 동기화, 서버 쪽 데이터 업데이트 등을 쉽게 만들어 주는 React 라이브러리입니다.

## 2. React Query 장점

- 간단하고 직관적인 API를 제공하여 데이터 가져오기, 캐싱, 업데이트, 백그라운드 동기화 등을 쉽게 처리
- 자동으로 데이터를 캐싱하고 관리하며, 데이터를 최신 상태로 유지합니다.
- 백그라운드 데이터 동기화를 통해 서버 데이터가 변경될 때 자동으로 클라이언트 상태를 업데이트
- 동일한 요청을 여러 번 보내지 않도록 서버 요청을 캐싱하고 재사용할 수 있어 네트워크 트래픽을 최소화
- 컴포넌트 내에서 동적으로 데이터 요청을 처리할 수 있으며, 동적 쿼리 키 및 변수를 사용하여 다양한 상황에 대응가능

## 3. React Query 설치 및 세팅

[@tanstack/react-query](https://www.npmjs.com/package/@tanstack/react-query)

```
npm i @tanstack/react-query
```

```
   import { QueryClient, QueryClientProvider } from 'react-query'

    const queryClient = new QueryClient()

    function App() {
      return
        <QueryClientProvider client={queryClient}>
        ...
        </QueryClientProvider>
    }
```

## 4. React Query Devtools

[React Query Devtools 공식문서](https://tanstack.com/query/latest/docs/react/devtools)

- react-query-devtools는 React Query 라이브러리와 함께 사용할 수 있는 개발 도구 확장입니다.
- 이 Devtools를 사용하면 React Query가 관리하는 데이터와 쿼리 상태를 시각적으로 모니터링하고 디버깅할 수 있어 애플리케이션의 상태와 데이터 요청을 보다 쉽게 이해하고 문제를 식별할 수 있습니다.

### 4-1. Install and Import the Devtools

```
npm i @tanstack/react-query-devtools
```

```
import { ReactQueryDevtools } from '@tanstack/react-query-devtools'
```

### 4-2. Floating Mode

"Floating Mode"은 개발 도구(Devtools)를 앱 내에서 고정된 부동 요소(fixed, floating element)로 마운트하고 화면의 모서리에 표시 및 숨기기 토글을 제공합니다. 이 토글 상태는 localStorage에 저장되어 새로 고침을 거친 후에도 기억됩니다.

```
import { ReactQueryDevtools } from '@tanstack/react-query-devtools'

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      {/* The rest of your application */}
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  )
}
```
