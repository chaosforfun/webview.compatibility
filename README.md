# ios 和 android 下各版本webview兼容性问题汇总

## android 4.4.x 下innerWidth bug

android 4.4.x 下 window.innerWidth 的值 ~~需要等到100ms后才正常~~ 一开始不正常，什么时候正常未知 ~~，之前是undefined~~;

## ios5 下 iframe 的viewport大小受scale影响

比如 iframe 设置宽640, 高100, scale(0.5)。在其他系统下 iframe内的viewport大小为 640*100，而ios5下 iframe 大小为320*50

## 所有ios版本下 给body元素添加click事件不起作用

比如
    
    <body>
        <div id="test1">test1</div>
        <div id="test2">test2</div>
    </body>
    
    
    body.addEventListener('click', function () {
        alert('body');
    })
    test2.addEventListener('click', function () {
        alert('test2')
    })
    
    
点击test1无反应，点击test2弹出 test2， body

## Android 4.4.x meta viewport 不起作用

需在创建webview的时候要添加配置
    
        this.appView.getSettings().setUseWideViewPort(true);
        this.appView.getSettings().setLoadWithOverviewMode(true);
        
## Android postMessage

在webview内打开本地文件A.html，在A内新建iframe  B

    B.html
    
        window.addEventListener("message", function (e) {
            e.source.postMessage('test', e.origin);
            //或者
            e.source.postMessage('test', 'file://);
        }, false)
        
会报错 
Uncaught SyntaxError: An invalid or illegal string was specified. 
或者
Unable to post message to file://. Recipient has origin null.

需要创建webview的时候添加配置

    webView.getSettings().setAllowFileAccessFromFileURLs(true);
    
看字面意思是关闭对本地加载的html文件的一些安全限制。

## sdk5 下 页面可能会一开始大小不对，然后变成对的
