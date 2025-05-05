---
title: react-native에서 코드 공통 적용을 위한 babel plugin 사용기
tags:
  - react
  - reactnative
  - "#babel"
draft: true
date: 2025-05-03 00:14
---

> 참조 : [toss tech Transpiler,"Transpiler, “사용”말고 “활용”하기"](https://toss.tech/article/27750)

## 목적

- 특정 태그(<Button /> , <TouchableOpacity />, <Pressable />) 등에 attribute 에 공통적으로 적용하고 싶은 속성을 따로 작성하지 않아도 일관된 기능이 적용되게 작동
- 하지만 일괄적용 시키고 싶지않은 부분들은 따로 명시 할경우 덮어 씌워지게 가능
- 여러 attribute 등을 넣고싶다!

## 사용 목적

- react native 에서 bable plugin을 사용하는 법을 알아보기 위해 간단히 확인할수있는 테스트를 만드려고 합니다.
- `<Pressable />`이라는 태그에 onLonPress와 delayLongPress 라는 attribute가 있는데 모든 `<Pressable />` 에 해당 attribute 를 넣어 일관된 동작을 넣으려고 합니다.
-
