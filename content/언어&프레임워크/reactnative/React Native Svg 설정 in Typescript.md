---
title: React Native Svg 설정 in Typescript
tags:
  - "#reactnative"
draft: false
date: 2025-05-03 00:14
---

## 참조 :
> [https://github.com/kristerkari/react-native-svg-transformer](https://github.com/kristerkari/react-native-svg-transformer)

- 특이사항 & 고려할 사항
  - RN 버전 , TypeScript 사용 유무
- react-native-svg-transformer 를 devDependencies에 설치

```jsx
yarn add react-native-svg
yarn add -D react-native-svg-transformer
```

## metro.config.js 설정
  - 특이사항 :
    - 해당 부분은 RN의 버전에 따라 설정하는것이 다르기때문에 공식문서를 보고 메뉴얼에 맞게 설정해야한다.
    - 필자는 0.72버전 이상의 RN 을 사용하여서 다음과같이 설정했다.
    ```jsx
    const { getDefaultConfig, mergeConfig } = require("@react-native/metro-config")

    const defaultConfig = getDefaultConfig(__dirname)
    const { assetExts, sourceExts } = defaultConfig.resolver

    /**
     * Metro configuration
     * <https://facebook.github.io/metro/docs/configuration>
     *
     * @type {import('metro-config').MetroConfig}
     */
    const config = {
      transformer: {
        babelTransformerPath: require.resolve("react-native-svg-transformer"),
      },
      resolver: {
        assetExts: assetExts.filter((ext) => ext !== "svg"),
        sourceExts: [...sourceExts, "svg"],
      },
    }

    module.exports = mergeConfig(defaultConfig, config)
    ```
- typescript decalare.d.ts 생성
  - TypeScript 사용시 임의로 svg type을 선언해주어야 한다.
  - CustomType의 위치와 이름은 자유롭게 설정해도 된다.
  - tsconfig.json 에 customType 명시
  - 예시)
  ```tsx
  // 위치 : ~/src/@types/declarations.d.ts

  declare module "*.svg" {
    import React from "react"
    import { SvgProps } from "react-native-svg"
    const content: React.FC<SvgProps>
    export default content
  }
  ```
  ```tsx
  {
    "extends": "@tsconfig/react-native/tsconfig.json",
    "compilerOptions": {
  		...
      "typeRoots": ["./node_modules/@types", "./src/@types"]
  		// ㄴ node_modules/@types는 필수적으로 명시해주고
  		// 그 다음에 svg 타입 선언을 위한 declar 파일위치를 명시해준다.
  		...
    }
  }
  ```
- metro재시작!!
  - 모든 설정을 마치고 metro 서버를 재시작 해주어야 정상적으로 읽어올 수 있다.
