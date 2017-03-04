---
title: git commit 规范
---

git commit 一些规范

<!--more-->

> [原文链接](http://karma-runner.github.io/0.10/dev/git-commit-msg.html)

## 提交commit的一些格式:
```
<type>(<scope>): <subject>

<body>

<footer>
```


## 消息头

- - - -
#### 通常第一行不会超过70个字符，第二行一般都是空白行，后面的每行都不应该超过80个字符

> *Allowed <type> values:*

* **feat** (新feature)
* **fix** (修复bug)
* **docs** (修改文档)
* **style** (css修改)
* **refactor** (代码重构)
* **test** (测试相关)
* **chore** (更新自动化任务，不修改业务代码)

> *Example <scope> values:*

* init
* runner
* watcher
* config
* web-server
* proxy
* etc.

The <scope> can be empty (eg. if the change is a global or difficult to assign to a single component), in which case the parentheses are omitted.


## 消息体
- - - -
* uses the imperative, present tense: “change” not “changed” nor “changes”
* includes motivation for the change and contrasts with previous behavior

For more info about message body, see:

* http://365git.tumblr.com/post/3308646748/writing-git-commit-messages
* http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html




