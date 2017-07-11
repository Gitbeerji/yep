### 问题描述

给页面中的div绑定click事件后，在手机端(IOS),点击时会有灰色半透明的背景，采用下面的方案，成功解决问题

### 网上找到的解决方案

1.做好的页面在手机端测试时，发现部分浏览器，tap后会出现一个半透明的灰色背景，（被批...），起初以为是outline作怪，加上后发现没反应，最后发现是tap后的背景高亮，要重设这个表现，则需要设置-webkit-tap-highlight-color为所需色彩，直接透明吧：


a,img,button,input,textarea{-webkit-tap-highlight-color:rgba(255,255,255,0);}

2.另外，如何去掉textarea,input的默认样式（IOS上的圆角及内阴影等，Android未测试）：




input,textarea{-webkit-appearance:none;}