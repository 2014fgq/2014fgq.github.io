# drawRect
- 每次系统调用drawRect方法之前,都会给drawRect方法传递一个跟当前view相关联上下文，因此只有在drawRect:方法中才能获取到上下文;

- 实现`- (void)drawRect:(CGRect)rect`方法,在drawRect:方法中取得上下文后，就可以绘制东西到view上；

- drawRect:方法在什么时候被调用？
    1. 当view第一次显示到屏幕上时（被加到UIWindow上显示出来）,只会调用一次（系统自动调用）

    - 调用view的`setNeedsDisplay`或者`setNeedsDisplayInRect:`时  ，根据需要调用
        - drawRect不能手动调用drawRect只能系统调用

- 每次调用drawRect，会先将之前的layer清空再重新渲染图案

##UIView图形显示实质
1. UIView自身是与用户交互，图形显示是由layer（图层）属性处  理的
2. layer（图层）分为主（根）层和content层，drawRect:方法中取得的是一个Layer Graphics Context的主层，因此绘制的东西其实是绘制到view的layer上去了


