---
title: git commit msg
---

git commit 一些规范

<!--more-->

> [原文链接](http://karma-runner.github.io/0.10/dev/git-commit-msg.html)

## Format of the commit message:
```
<type>(<scope>): <subject>

<body>

<footer>
```


## Message subject (first line)
- - - -
#### First line cannot be longer than 70 characters, second line is always blank and other lines should be wrapped at 80 characters.
 
> *Allowed <type> values:*  

* **feat** (new feature)
* **fix** (bug fix)
* **docs** (changes to documentation)
* **style** (formatting, missing semi colons, etc; no code change)
* **refactor** (refactoring production code)
* **test** (adding missing tests, refactoring tests; no production code change)
* **chore** (updating grunt tasks etc; no production code change)

> *Example <scope> values:*  

* init
* runner
* watcher
* config
* web-server
* proxy
* etc.

The <scope> can be empty (eg. if the change is a global or difficult to assign to a single component), in which case the parentheses are omitted.


## Message body
- - - -
* uses the imperative, present tense: “change” not “changed” nor “changes”
* includes motivation for the change and contrasts with previous behavior

For more info about message body, see:

* http://365git.tumblr.com/post/3308646748/writing-git-commit-messages
* http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html




