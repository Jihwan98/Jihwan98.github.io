---
title: "[VS Code] Prettier 일부 파일 예외처리 방법"
author: JIHWAN PARK
categories: [ETC, Environment]
tag: [VS Code, Prettier]
math: true
date: 2023-07-09 13:50:53 +0900
last_modified_at: 2023-07-09 13:50:53 +0900
---
> VS Code에서 일부 파일들은 Prettier가 적용되지 않도록 예외처리하는 방법입니다.
{: .prompt-info}

## 🔎 문제 상황

Prettier가 특정 파일에는 적용되지 않도록 하길 바랬음

예를 들어, 특히 마크다운 작성 할 때 공백을 자동으로 제거하는 등 내가 원하는 대로 작성이 안되었음

## ✅ 해결 방법
root directory에 `.prettierignore` 파일을 생성하고 `*.md`를 입력해주면 된다.

```
// .prettierignore

*.md
```