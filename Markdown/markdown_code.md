# Markdown 嵌入代码

## 单行代码嵌入
如果是段落上的一个函数或片段的代码可以用反引号把它包起来（`），例如：
```markdown
`printf()` 函数
```
显示效果:  
`printf()` 函数

## 代码块嵌入
使用 ``` 包裹一段代码，并指定一种语言（也可以不指定）：
```markdown
    ```javascript
    $(document).ready(function () {
        alert('RUNOOB');
    });
    ```
```
显示效果:
```javascript
$(document).ready(function () {
    alert('RUNOOB');
});
```

