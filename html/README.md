
> 在线HTMl编辑器

## 项目信息



## 功能特点

* 高性能代码编辑框
* 最高支持400万行代码编辑
* 支持代码高亮
* 支持代码行数显示
* 支持代码智能查错
* 支持代码自动补全
* 支持字号调节
* 支持主题切换
* 支持实时存储到浏览器本地存储
* 支持一键复制代码
* 支持一键清空代码
* 支持恢复上次编辑内容及光标位置
* 支持一键HTML转换文本
* 支持一键下载代码文件


## 用到的库

* Bootstrap
* jQuery.js
* clipboard.js
* Ace编辑器
* FileSaver.js

## 代码片段

### 编辑器配置

```javascript
// 新建编辑器
var editor = ace.edit("editor")
// 开始配置编辑器
ace.config.set("basePath", 'js')
// 默认主题
editor.setTheme("ace/theme/chrome")
var htmlMode = ace.require("ace/mode/html").Mode
editor.session.setMode(new htmlMode())
ace.require("ace/ext/language_tools")
editor.setOptions({
    enableBasicAutocompletion: true,
    enableSnippets: true,
    enableLiveAutocompletion: true
})
// 设置编辑器字体大小
var mr_setFontSize = localStorage.PonConHtmlEditorsetFontSize
if (!mr_setFontSize) {
    mr_setFontSize = 18
}
editor.setFontSize(parseInt(mr_setFontSize))
$("label.label-fontsize").html('字号：' + mr_setFontSize)
$("#set-fontsize").val(mr_setFontSize)
if (localStorage.PonConHtmlEditorCode) {
    editor.setValue(localStorage.PonConHtmlEditorCode)
}
// 消除文本选中状态
var His_row = localStorage.PonConHtmlEditorCursorRow
var His_column = localStorage.PonConHtmlEditorCursorColumn
if (!His_row) {
    His_row = 0;
}
if (!His_column) {
    His_column = 0;
}
editor.gotoLine(His_row - 1)
editor.moveCursorTo(His_row, His_column)
editor.setShowPrintMargin(true)
// 以上为编辑器配置
```

### 在框架中预览HTML网页

```javascript
$('#viewer').html('<iframe id="iframe" name="iframe" style="width: 100%; height: 100%;"></iframe>')
var iframe = window.frames['iframe']
iframe.document.open()
iframe.document.write(editor.getValue())
iframe.document.close()
```

### 功能按钮事件

```javascript
// 复制代码
$('.btn-group button.toCopy').click(function () {
    $('button.copy').attr('data-clipboard-text', editor.getValue()).click().attr('data-clipboard-text', '')
})
// 清空代码
$('.btn-group button.clean').click(function () {
    if (confirm('确定要清空？')) {
        editor.setValue('')
    }
})
// 下载代码
$('.btn-group button.download').click(function () {
    var blob = new Blob([editor.getValue()], { type: "text/plain;charset=utf-8" })
    saveAs(blob, new Date().getTime() + ".txt")
})
// HTML转文本
$('.btn-group button.toText').click(function () {
    $('#toTextResult').html($(editor.getValue()).text().replace(/\n/g, '<br />'))
})
```
### 实时保存

```javascript
// 定时保存
setInterval(function () {
    localStorage.PonConHtmlEditorCode = editor.getValue()
    localStorage.PonConHtmlEditorCursorColumn = editor.selection.getCursor().column
    localStorage.PonConHtmlEditorCursorRow = editor.selection.getCursor().row
}, 500)
```
