---
layout: post
title: "콘솔에 색 입히기"
category: [dev]
---

터미널에서 출력할 때, 색을 입히고자 하는 스트링 앞에 `"\x1b[30m"`와 같은 **ANSI escape code**를 이용할 수 있다. 예를 들면 FAIL을 빨강으로 표시하기 위해 `"\x1b[31mFAIL\x1b[37m"` 라고 써줄 수 있다. FAIL 앞에 빨강 코드를 써 주고, 바로 뒤에는 흰색 코드를 써서 다른 글씨들은 정상적으로 흰색으로 보이도록 한다.

### **대표적인 색깔 목록**

    Reset = "\x1b[0m";
    Bright = "\x1b[1m";
    Dim = "\x1b[2m";
    Underscore = "\x1b[4m";
    Blink = "\x1b[5m";
    Reverse = "\x1b[7m";
    Hidden = "\x1b[8m";
    FgBlack = "\x1b[30m";
    FgRed = "\x1b[31m";
    FgGreen = "\x1b[32m";
    FgYellow = "\x1b[33m";
    FgBlue = "\x1b[34m";
    FgMagenta = "\x1b[35m";
    FgCyan = "\x1b[36m";
    FgWhite = "\x1b[37m";
    BgBlack = "\x1b[40m";
    BgRed = "\x1b[41m";
    BgGreen = "\x1b[42m";
    BgYellow = "\x1b[43m";
    BgBlue = "\x1b[44m";
    BgMagenta = "\x1b[45m";
    BgCyan = "\x1b[46m";
    BgWhite = "\x1b[47m";
