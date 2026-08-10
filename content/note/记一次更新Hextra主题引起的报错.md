+++
date = '2026-08-09T19:45:40+08:00'
draft = false
title = '记一次更新 Hextra 主题引起的报错'
description = 'CRLF 导致的 comment ends before closing delimiter'
+++

Hextra `v0.12.0` -> `v0.12.3` 后报错：

```
"[我的 Hugo 项目根目录]\themes\hextra\layouts_shortcodes\gallery-item.html:1:1": parse of template failed: template: _shortcodes/gallery-item.html:1: comment ends before closing delimiter
```



### 问题分析

报错文件内容：
```
{{- /*
  ...注释内容...
*/
-}}
...
```

根据报错信息定位到 Go 模板解析器源码 [lex.go](https://github.com/golang/go/blob/master/src/text/template/parse/lex.go#L353)：

关键代码：

```go
const (
	defaultLeftDelim  = "{{"
	defaultRightDelim = "}}"
	leftComment       = "/*"
	rightComment      = "*/"
)

const (
	spaceChars    = " \t\r\n"  // These are the space characters defined by Go itself.
	trimMarker    = '-'        // Attached to left/right delimiter, trims trailing spaces from preceding/following text.
	trimMarkerLen = Pos(1 + 1) // marker plus space before or after
)

// lexComment scans a comment. The left comment marker is known to be present.
func lexComment(l *lexer) stateFn {
	l.pos += Pos(len(leftComment))
	x := strings.Index(l.input[l.pos:], rightComment)
	if x < 0 {
		return l.errorf("unclosed comment")
	}
	l.pos += Pos(x + len(rightComment))
	delim, trimSpace := l.atRightDelim()
	if !delim {
		return l.errorf("comment ends before closing delimiter")
	}
	...
}

// atRightDelim reports whether the lexer is at a right delimiter, possibly preceded by a trim marker.
func (l *lexer) atRightDelim() (delim, trimSpaces bool) {
	if hasRightTrimMarker(l.input[l.pos:]) && strings.HasPrefix(l.input[l.pos+trimMarkerLen:], l.rightDelim) { // With trim marker.
		return true, true
	}
	if strings.HasPrefix(l.input[l.pos:], l.rightDelim) { // Without trim marker.
		return true, false
	}
	return false, false
}

func hasRightTrimMarker(s string) bool {
	return len(s) >= 2 && isSpace(rune(s[0])) && s[1] == trimMarker
}

// isSpace reports whether r is a space character.
func isSpace(r rune) bool {
	return r == ' ' || r == '\t' || r == '\r' || r == '\n'
}
```

由此可知报错原因：

Go 模板解析器只允许以下 2 种注释右定界符格式：
```
*/}}
*/[一个空白字符]-}}
```

Hextra 仓库原文件是 LF（`\n`）换行，`*/` 换行后接 `-}}` 是合法的．

但如果 Git 配置了 `core.autocrlf=true`（Windows 安装 Git 时的默认选项），checkout 时文件会被转换成 CRLF（`\r\n`）换行，`*/` 和 `-}}` 之间变成了两个空白字符，于是就会报 `comment ends before closing delimiter` ．



### 解决方法

#### 方法 1：单独修改主题

```bash
git -C themes/hextra config core.autocrlf input
git -C themes/hextra rm -r --cached .
git -C themes/hextra checkout -- .
git -C themes/hextra reset --hard
```

#### 方法 2：全局修改

```bash
git config --global core.autocrlf input
git submodule deinit -f themes/hextra
git submodule update --init --recursive
```


---

> 其实后来还是退回到 v0.12.0 了
> 感觉还是老版本的界面风格好看点
