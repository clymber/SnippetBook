# Markdown 嵌入图片
## Markdown 图片基本语法格式
```markdown
![alt 属性文本](图片地址)
![alt 属性文本](图片地址 "可选标题")
```
语法详解:
1. 开头一个感叹号 !
2. 接着一个方括号，里面放上图片的替代文字
3. 接着一个普通括号，里面放上图片的网址，最后还可以用引号包住并加上选择性的 'title' 属性的文字。
例子:
```markdown
![Desktop picture](./mydesktop.png)
 OR:  
![RUNOOB 图标](http://static.runoob.com/images/runoob-logo.png "RUNOOB")
```
效果展示:
![Desktop picture](./mydesktop.png)
 OR:  
![RUNOOB 图标](http://static.runoob.com/images/runoob-logo.png "RUNOOB")


## 调整图片大小
Markdown 还没有办法指定图片的高度与宽度，如果你需要的话，你可以使用普通的标签。
```markdown
<img src="./mydesktop.png" width="50%">
```
效果展示:  
<img src="./mydesktop.png" width="50%">

```markdown
<img src="mydesktop.png" alt="mydesktop" style="zoom:20%;" />
```

显示效果:

<img src="mydesktop.png" alt="mydesktop" style="zoom:20%;" />
