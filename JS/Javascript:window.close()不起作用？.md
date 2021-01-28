一般的窗口关闭的JS如下写法：

window.close()

但是呢，chrome，firefox等中有时候会不起作用。

改为下面的写法：

window.open("about:blank","_self").close()

或者

window.open("","_self").close()

如果是frame的时候如下写法：

一般：window.top.close()

改善：window.open("about:blank","_top").close()   或者 window.open("","_top").close()

其他比如window.parent.close()也是可以用类似的方法。

如果关闭按钮既可能是单独的画面，也可能是frame的一部分的时候，可以用下面的写法对应。

function closeWin() {
   try {
       window.opener = window;
       let win = window.open("","_self");
       win.close();
       //frame的时候
       top.close();
   } catch (e) {

}
}

