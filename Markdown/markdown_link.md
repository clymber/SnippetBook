# Markdown 嵌入链接
## Markdown 嵌入链接的基本语法
链接使用方法如下：
```markdown
[链接名称](链接地址)
或者
<链接地址>
```
例1:
```markdown
这是我的GitHub: [My GitHub](https://github.com/SupperDragon/SnippetBook)
```
显示效果:  
这是我的GitHub: [My GitHub](https://github.com/SupperDragon/SnippetBook)

例2:  
`<https://github.com/SupperDragon/SnippetBook>`
显示效果:  
<https://github.com/SupperDragon/SnippetBook>`

## Markdown 嵌入链接的高级语法
```markdown
链接也可以用变量来代替，文档末尾附带变量地址：
这个链接用 1 作为网址变量 [Google][1]
这个链接用 runoob 作为网址变量 [Runoob][runoob]
然后在文档的结尾为变量赋值（网址）

  [1]: http://www.google.com/
  [runoob]: http://www.runoob.com/
```
显示效果:  
链接也可以用变量来代替，文档末尾附带变量地址：  
这个链接用 1 作为网址变量 [Google][1]  
这个链接用 runoob 作为网址变量 [MyGigHub][MyGigHub]  
然后在文档的结尾为变量赋值（网址）





[1]: http://www.google.com/
[MyGigHub]: https://github.com/SupperDragon/SnippetBook

