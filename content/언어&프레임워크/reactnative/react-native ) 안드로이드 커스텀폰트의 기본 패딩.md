---
title: react-native ) 안드로이드 커스텀폰트의 기본 패딩
tags:
  - "#reactnative"
  - "#reactnative/android"
draft: false
date: 2025-05-03 00:14
---

## 상황 :

- 외부의 커스텀 폰트를 적용 시켰더니 line-height 를 적용 시키지 않았는데 글자간의 상하 간격이 크게 차이난다.
- 결정적으로 ios와 스타일 통일이 안됨

## 원인 :

- 안드로이드 TextView 기본값이 참인 `includeFontPadding` 속성이 적용되는 문제

## 해결 :

- style에 \*\*`include-font-padding: false` 를 선언 해주면 된다.

```tsx
// ex) styled-components
export const Label = styled.Text`
  include-font-padding: false;
	// ...
`;

// ex) styleSheet
const styles= styleSheet.create({
	text: {
		includeFontPadding: false;
	}
})
```
