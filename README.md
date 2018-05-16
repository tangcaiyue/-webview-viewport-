# -webview-viewport-
webview 引入了一个html，发现viewport无效，背景图片很大在安卓下会出现滚动条，而在ios里面会自动缩小展示，想实现安卓也像苹果那样自动缩小显示，但是调了viewport一直没有效果

网上查找了发现文章说：对于ios设备，设置width可以生效，但对于android，设置width并不会生效。ios设备，缩放的比率即dpi是通过你设置的width和设置真实分辨率自动计算的，而android下你设置width无效，也就是说，有三个变量：浏览器width、设备真实width、dpi。 我们简单地用个公式来表达它们之间的关系吧（并非真实关系，简单说明用） 设备真实width * dpi = 浏览器width，这里的三个变量，设备真实width是个我们不能操作的已知值，另外两个变量我们可以设置一个来影响另一个，在ios中，我们能改的是浏览器width，dpi自动生成，而在android中，我们能改的是dpi，浏览器width自动生成。对于android，无论我们如何设置width，也不会对浏览器width产生影响。
ios设备在横竖屏时，会自动调整dpi，无论横屏还是竖屏，都能保证浏览器width等于viewport中设置的值，所以横竖屏的时候，页面里显示的内容的大小是会自动缩放产生变化的。而android手机在横竖屏的时候，不会改变dpi，在横竖屏的时候，网页不会产生缩放。也正因此，ios可以保证横竖屏页面都不会产生滚动条，满屏显示。

网上找了很久，终于发现一篇有效，链接为：https://github.com/mishe/blog/issues/152
然后我自己稍微改了一下w1的方法：
html在onload调用w1方法
    document.write('<meta id="vp" name="viewport" content="width=device-width,initial-scale=0.5' +
       ',user-scalable=yes">')

然后在webview加 scalesPageToFit={Platform.OS === 'ios'? true : false}
  <WebView
      ref={"webview"}
       onLoadEnd={() => {
            this.loadData();
       }}
       style={{height: 100}}
       source={source}
       scalesPageToFit={Platform.OS === 'ios'? true : false}
   >
   基本实现了我的需求 ，第一次写这个文章，真是尴尬，没有期望谁来看，我自己看就好了，这个安卓的webwiew viewport的问题真的找了很久
