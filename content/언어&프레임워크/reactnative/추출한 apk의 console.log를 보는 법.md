---
title: 추출한 apk의 console.log를 보는 법
tags:
  - android
  - reactnative
draft: false
date: 2025-05-03 00:14
---

> 참조 : [How to do logging in React Native](https://stackoverflow.com/questions/30115372/how-to-do-logging-in-react-native)

- 실행 하고자하는 device가 해당 termial을 실행시키는 기기와 연결이 되어있어야 한다.
- debug가 아닌 apk 파일이 있고 해당 프로젝트에 적용된 console.log, consolw.warn 등을 보고싶을때 terminal에서 다음과 같은 커맨드를 입력하면 된다.

```bash
npx react-native log-ios
npx react-native log-android
```
