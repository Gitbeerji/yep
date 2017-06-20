# 鼠标悬浮模糊效果任务学习总结
## 任务地址：    [有趣的鼠标悬浮模糊效果](http://ife.baidu.com/course/detail/id/14)

### 语义化标签figure

figure标签规定独立的流内容（图像、图表、照片、代码等）
figure元素的内容应该与主内容先关，但如果被删除，则不应对文档流产生影响
figure标签是HTML5中的新标签（IE8 以及更早的版本不支持）

常用形式：
```html
	<figure>
		<figcaption>Welcome</figcaption>
		<img src="www.jpg" />
	</figure>
```

### flex布局(Flexible Box) 弹性布局 [Flex布局教程](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

任何一个容器都可以指定为Flex布局
```css
	.box {
		display: flex;
		display: -webkit-flex;
	}
	
	.box {
		display: inline-flex;
	}
```
设置Flex布局之后，子元素的 float clear vertical-align 属性都将无效

简要说明:

以下6个属性设置在容器上
- flex-direction 决定主轴的方向（项目的排列方向）
- flex-wrap	定义一条轴线排不下如何换行
- flex-flow  flex-direction与flex-wrap的简写形式
- justify-content 定义子元素在主轴上的对齐方式
- align-items 定义子元素在交叉轴上如何对齐	
- align-contnet 定义多根线轴的对齐方式

以下6个属性设置在项目上
- order		定义项目的排列顺序
- flex-grow		定义项目的放大比例
- flex-shrink	定义项目的缩小比例
- flex-basis	定义在分配多余空间之前，项目占据的主轴空间
- flex			flex-grow,flex-shrink,flex-basis的简写
- align-self	允许单个项目与其他项目不一样的对齐方式

### transition过渡
transition是简写属性，用于设置四个过渡属性
transition: property duration timing-function delay
- transition-property 设置过渡效果的CSS属性的名称
- transition-duration 完成过渡效果需要多少秒或毫秒
- transition-timing-function 速度效果的速度曲线
- transition-delay	定义过渡效果何时开始

### 动画效果
![img](https://ww3.sinaimg.cn/large/006tNbRwly1fcr5jmrmujg30cj06xqv5.gif)  

上图所示的动画效果中边框从中间向两边延伸的实现方式为
1. 边框是使用元素的伪元素来实现的，after，before
1. 一开始设定 top: 50%;height: 0;visibility: none;
1. 鼠标移入后 top: 0;height: 100%;visibility: visibile;

### 字体流光效果
字体背景使用过渡颜色，然后使背景向右循环动起来，达到流光效果

### 毛玻璃的效果
使用 CSS3 filter属性 注释要运用于图片，实现一些特效
- grayscal 灰度
- sepia 褐色
- saturate 饱和度
- hue-rotate 色相旋转
- invert 反色
- opacity 透明度
- brightness 亮度
- contrast 对比度
- blur 模糊
- drop-shadow 阴影
