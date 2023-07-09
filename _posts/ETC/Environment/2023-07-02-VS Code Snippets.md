---
title: "[VS Code] Custom 자동 완성, Snippets "
author: JIHWAN PARK
categories: [ETC, Environment]
tag: [VS Code, Snippets]
math: true
date: 2023-07-02 21:20:27 +0900
last_modified_at: 2023-07-02 21:20:27 +0900
---

> VS Code에서 Custom 자동 완성(Code Snippets)을 만드는 방법을 소개합니다.
{: .prompt-info}

Github Blog를 작성하다 보면 동일한 구조를 작성하는 경우가 많다. 예를 들어서 코딩테스트 관련 게시글의 경우 동일한 양식으로 작성을 하고 있고, 블로그 글의 정보를 나타내는 Header 부분이라던지, Header 부분의 현재 시간을 적을 때라던지 등의 경우가 있다.

이럴 때 VS Code의 Snippets을 활용하면 편리하게 사용할 수 있다.

## 📄 Snippets: Configure User Snippets > markdown.json

- `Ctrl + Shift + P`를 눌러 `Snippets` 을 입력하고 `Snippets: Configure User Snippets` 을 선택
- 그리고 `markdown.json`을 선택
- 여기에 아래와 같이 원하는 것을 작성하면 된다.
- 자세한 문법은 [VS Code 공식문서](https://code.visualstudio.com/docs/editor/userdefinedsnippets)와 구글링을 통해 작성 해보기!

```json
{
  "Print Date": {
    "prefix": "now",
    "body": [
      "$CURRENT_YEAR-$CURRENT_MONTH-$CURRENT_DATE $CURRENT_HOUR:$CURRENT_MINUTE:$CURRENT_SECOND +0900"
    ],
    "description": "Print date"
  },
  "Post Head": {
    "prefix": "post",
    "body": [
      "---",
      "title:",
      "author: JIHWAN PARK",
      "categories: []",
      "tag: []",
      "math: true",
      "date: $CURRENT_YEAR-$CURRENT_MONTH-$CURRENT_DATE $CURRENT_HOUR:$CURRENT_MINUTE:$CURRENT_SECOND +0900",
      "last_modified_at: $CURRENT_YEAR-$CURRENT_MONTH-$CURRENT_DATE $CURRENT_HOUR:$CURRENT_MINUTE:$CURRENT_SECOND +0900",
      "---",
      "> ",
      "{: .prompt-info}"
    ],
    "description": "Print date"
  }
}
```

- `prefix` : 설정할 단축키
- `body` : 자동 완성할 내용
- `description` : 설명
- ex) `now`를 치고 `Ctrl + Space`를 누르면 현재 시간이 작성됨

## 📄 settings.json

- `prefix`를 치고 탭이나 엔터만 눌러도 완성되게 하려면 아래와 같은 설정을 추가하면 된다.
- 마찬가지로 `Ctrl + Shift + P`를 누르고 `settings.json`을 검색하고 `Preferences: Open User Settings (JSON)`을 열고 아래의 내용을 추가한다.

```json
{
  "[markdown]": {
    "editor.quickSuggestions": {
      "other": "on",
      "comments": "on",
      "strings": "on"
    }
  }
}
```

<details>
<summary>🔎 뭐가 다른지는 모르겠음..</summary>
<div markdown="1">

- 참고한 블로그에 보면 이렇게 작성하면 된다고 해서 해보면 되는데, 밑줄이 그어져서 불편..
- 그래서 그냥 자동완성 하면 위에 코드처럼 되길래 위에 처럼 작성해서 쓰고있음.

```json
{
  "[markdown]": {
    "editor.quickSuggestions": true
  }
}
```

</div>
</details>

---

📜 참고한 자료들
: 
- [VS Code 공식 문서](https://code.visualstudio.com/docs/editor/userdefinedsnippets)
- [[VS Code] 커스텀 자동완성, Snippets](https://ansohxxn.github.io/vs/snippets/)
- [vscode 꿀팁 1 — 나만의 스니펫 사용하기](https://junewookang.medium.com/vscode-%EA%BF%80%ED%8C%81-1-%EB%82%98%EB%A7%8C%EC%9D%98-%EC%8A%A4%EB%8B%88%ED%8E%AB-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-28b6044a77d3)
