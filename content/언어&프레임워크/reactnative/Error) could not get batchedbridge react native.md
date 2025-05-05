---
title: Error) could not get batchedbridge react native
tags:
  - reactnative
  - error
draft: false
date: 2025-05-03 00:14
---

> 참조 : [Could not get BatchedBridge, make sure your bundle is packaged correctly](https://hwangtaehyun.github.io/blog/react-native/bundle-error/)

## 원인 :

- native 모듈 설치 이후 해당 모듈 사용 metro 서버에 캐시되어있던 데이터와 충돌이 남

## 해결 :

- metro를 캐시 초기화 하여 새로 시작

```jsx
npx react-native start --reset-cache
```
