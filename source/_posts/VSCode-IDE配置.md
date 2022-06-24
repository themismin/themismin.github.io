---
title: VSCode-IDE配置
tags:
  - VSCode
  - IDE
categories:
  - IT
  - VSCode
abbrlink: 1ebbfc56
date: 2022-06-23 20:56:43
---

## 插件
### Chinese (Simplified) (简体中文) Language Pack for Visual Studio Code
此中文（简体）语言包为 VS Code 提供本地化界面。

### Sublime Text Keymap and Settings Importer
导入Sublime Text设置和按键绑定到VS Code中。

### PHP Intelephense#
PHP 的代码提示、补全、跳转定义、格式化插件，功能强大，无需配置

### PHP DocBlocker
注释自动生成器

### PHP Snippets from PHPStorm
快捷代码片段

### Better Align
对齐代码
Code > 首选项 > 键盘快捷方式 > 打开键盘快捷方式(JSON)
// 将键绑定放在此文件中以覆盖默认值
[
    {
        "key"    : "shift+alt+d",
        "command": "wwm.aligncode",
        "when"   : "editorTextFocus && !editorReadonly"
    }
]
## 快捷键
```
shift+alt+d: 光标移动到等号行，等号对齐
shift+alt+f: 代码格式化

cmd+d     : 同时选中下一个同样单词
ctrl+cmd+g: 选中与当前选中字符相同的所有字符

ctrl+空格: 代码提示
```