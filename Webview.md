###WebView
好久没写博客了，项目中用到的 webview的东西比较多，在这里总结一下好了。
####权限
 在AndroidManifest.xml 中添加网络访问权限：<code>"android.permission.INTERNET"</code>
####设置WevView要显示的网页
 互联网用：<code>webView.loadUrl("http://www.google.com");</code>
          本地文件用：<code>webView.loadUrl("file:///android_asset/XX.html");</code>
####函数
shouldOverrideUrlLoading:拦截 url 跳转,在里边添加点击链接跳转或者操作
onPageStarted:页面开始加载时
onPageFinished:页面加载结束时
onLoadResource:加载资源
onReceivedError:加载失败时，可以设置加载失败时 reload

addJavascriptInterface:提供 js 可以调用的函数接口

setJavaScriptEnable:设置可以访问 javaScript
setBuiltInZoomControls:设置可以缩放

onKeyDown(int keyCoder,KeyEvent event)：处理 back 键
    
      public boolean onKeyDown(int keyCoder,KeyEvent event){
      if(webView.canGoBack() && keyCoder ==KeyEvent.KEYCODE_BACK{
         webview.goBack();   //goBack()表示返回webView的上一页面
          return true;
        }
      return false;
      }
####两种加载网页的方式
loadUrl():webView.loadUrl("http://www.baidu.com");
loadData():webView.loadUrl("file:///android_asset/xx.html");
loadData(String data, String mimeType, String encoding):data 为 html 代码内容

     String summary = "<html><body>You scored <b>192</b> points.</body</html>"
     webview.loadData(summary, "text/html", "UTF-8");
####WebViewClient （未完）

####WebChromeClient（未完）

####跳转到浏览器

    Uri uri = Uri.parse("http://www.example.com");
    Intent intent = new Intent(Intent.ACTION_VIEW, uri);
    startActivity(intent);
