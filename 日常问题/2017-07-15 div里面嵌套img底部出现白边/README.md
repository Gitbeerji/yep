### div里嵌套了img底部会出现白边

因为img默认是按基线(baseline)对齐的、对比下面图片中p,q,y等字母，你会发现三个字母的小尾巴和图片下方的空白一样高，下面这张图中的黑线就是那条基线

![Markdown](http://i2.kiimg.com/1949/2d5de6d23916543f.png)


要去掉空格可以使用vertical-align: bottom 或将img标签变为块级元素

下图将解释什么是基线

![Markdown](http://i1.buimg.com/1949/61b3cc6563b2de4c.png)



#### 其他方法

1. 将img图片display: block;
2. 将div的line-height设置的足够小
3. 将font-size设为0，实际上也是改变了line-height
4. 改变vertical-align，让它不是baseline，比如设置vertical-align: middle


#### 附带知识   

以下是张鑫旭讲解的line-height和vertical-align的视频      

[line-height](http://www.imooc.com/view/403)            
[vertical-align](http://www.imooc.com/learn/542)