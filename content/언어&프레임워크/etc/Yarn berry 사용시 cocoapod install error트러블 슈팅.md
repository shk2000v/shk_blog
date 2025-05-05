---
title: Yarn berry 사용시 cocoapod install error트러블 슈팅
tags:
  - "#package-manger"
  - error
draft: false
date: 2025-05-03 00:14
---

# Issue Image

> 대략 다음과 같은 에러 로그를 볼 수있다.

```zsh
✖ Installing CocoaPods dependencies (this may take a few minutes)
error
[!] Invalid `Podfile` file: [!] /usr/local/bin/node -p require.resolve(
    "react-native/scripts/react_native_pods.rb",
    {paths: [process.argv[1]]},
  ) /Users/user/Documents/sources/AwesomeProject/ios

node:internal/modules/cjs/loader:1080
  throw err;
  ^

Error: Cannot find module 'react-native/scripts/react_native_pods.rb'
Require stack:
- /Users/user/Documents/sources/AwesomeProject/ios/[eval]
    at Module._resolveFilename (node:internal/modules/cjs/loader:1077:15)
    at Function.resolve (node:internal/modules/cjs/helpers:127:19)
    at [eval]:1:9
    at Script.runInThisContext (node:vm:123:12)
    at Object.runInThisContext (node:vm:299:38)
    at node:internal/process/execution:79:19
    at [eval]-wrapper:6:22
    at evalScript (node:internal/process/execution:78:60)
    at node:internal/main/eval_string:28:3 {
  code: 'MODULE_NOT_FOUND',
  requireStack: [ '/Users/user/Documents/sources/AwesomeProject/ios/[eval]' ]
}

Node.js v18.17.1
.

 #  from /Users/user/Documents/sources/AwesomeProject/ios/Podfile:2
 #  -------------------------------------------
 #  # Resolve react_native_pods.rb with node to allow for hoisting
 >  require Pod::Executable.execute_command('node', ['-p',
 #    'require.resolve(
 #  -------------------------------------------
✖ Installing CocoaPods dependencies (this may take a few minutes)
error Looks like your iOS environment is not properly set. Please go to https://reactnative.dev/docs/environment-setup?os=macos&platform=android and follow the React Native CLI QuickStart guide for macOS and iOS.
info Run CLI with --verbose flag for more details.
```

# 원인

- 참조 할 수 있는 node_modules 가 없어서 생기는 에러이다.
- npm으로 한다면 볼 일 없는 문제지만 yarn 4버전으로 시작하다보니 발생하는 문제
- 참조하고자하는 node_modules이 생성되지 않아서 생긴 문제이다.

# 해결

> `yarn set version berry`

- yarn set version berry를 root project 터미널에 입력
- 그후 `.yarnrc.yml` 이 생성된다.
- 해당 파일에서 nodeLinker가 `nodeLinker: node-modules` 로 되어있는지 확인하고 없다면 추가해주면 된다.

```yml
nodeLinker: node-modules
yarnPath: .yarn/releases/yarn-4.6.0.cjs
```

---

# 토막 정보

yarnrc는 yarn의 내부 설정을 관리하는 파일이다.

- yarnPath: yarn을 프로젝트 내에 설치하는데 가장 선호되는 방법으로 모든 팀원이 같은 yarn version을 사용할 수 있도록 해준다.
- nodeLinker: Node 패키지를 설치하는데 어떤 링커를 사용할지 명시하는 속성. (`pnp`, `pnpm` ,`node-modules` 가능)

출처: [https://nukw0n-dev.tistory.com/37](https://nukw0n-dev.tistory.com/37) [찐이의 개발 연결구과:티스토리]
