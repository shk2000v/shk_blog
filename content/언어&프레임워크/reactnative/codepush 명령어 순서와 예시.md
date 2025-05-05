---
title: codepush 명령어 순서와 예시
tags: 
draft: true
date: 2025-05-03 00:14
---
> [!warning] 작성중인 문서
> 해당 문서는 codepush가 종료되어 유효하지 않아 새로운 문서로 대체 필요함 



## 코드푸시 create-history

```zsh
npx code-push create-history
```

## 코드푸시 rlease

```zsh
npx code-push release -b 2.1.99 --app-version 16.0.1 \
                      --platform ios --identifier prod --entry-file index.js \
                      --mandatory true

```
