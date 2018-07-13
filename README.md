# Objc_Framework
整个项目打包framework || 打包Framework

注意：
---
 1. Appdelegate 不能打包到framework 里面，所以AppDelegate.m文件在编译的文件列表中要撤选,在AppDelegate中做的一些操作要全部封装成接口暴露出去
 2. 项目中使用的三方框架最好是换成cocoapods管理
 3. .默认情况下，只会打包代码文件,像图片,xib,sb,plist文件等都不会被打包,这时这些资源文件即使打成framework也不会被成功使用 ，这就需要创建一个bundle文件夹,把你所有的图片,plist文件,Images.xcassets,xib,storyboard都会打包进去,而且项目中用到这些资源文件的时候,只能通过bundle/xxx.png文件名的方式来设置(如果允许，可以直接把图片等资源直接放到外面就可以直接调用)

## 第一步 
先创建一个静态库工程，选择framework
![创建framework](https://img-blog.csdn.net/20180627140130602?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzIzOTA3NDY3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## 第二步
 把项目中使用的三方框架 用cocoapods 生成 这里添加一些常用的三方框架 
 ![cocoapods添加三方框架](https://img-blog.csdn.net/2018062714042216?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzIzOTA3NDY3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

pod install 之后生成的项目 打开就是这个样子
![这里写图片描述](https://img-blog.csdn.net/2018062714093913?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzIzOTA3NDY3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## 第三步
把需要打包的项目文件直接拖进来
![拖进来](https://img-blog.csdn.net/20180627141840584?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzIzOTA3NDY3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

然后创建一个单利或者是你自己写好的方法也是可以的
![这里写图片描述](https://img-blog.csdn.net/20180627141956859?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzIzOTA3NDY3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 - 框1：这里是你拖进来将要打包成framework的项目文件
 - 框2：这个按着原本项目的配置即可
 - 框3：原本工程使用的库文件会自动添加到这里，但不会包含三方框架
 - 框4：framework支持真机就选择真机，支持模拟器就选择模拟器，两个都支持合并即可

![这里写图片描述](https://img-blog.csdn.net/20180627142356433?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzIzOTA3NDY3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## 第四步
	## Build Settings ##
配置静态库支持架构，在Build Settings中,我这里只配置了真机的架构,如果需要支持模拟器的架构,可以自行添加进去.
![这里写图片描述](https://img-blog.csdn.net/2018062714303535?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzIzOTA3NDY3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

然后把framework设置成静态库,默认状态创建的framework其实是动态库,Mach-O Type 修改为Static Library
![这里写图片描述](https://img-blog.csdn.net/20180627143431455?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzIzOTA3NDY3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

然后创建一个bundle ，把项目中所有用到的图片都拖进去（请看注意事项第4点）

![这里写图片描述](https://img-blog.csdn.net/20180627144024838?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzIzOTA3NDY3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

	## Build Phases ##

 1. 框1 这个是你编译成framework的文件，如果有外部文件或者是单独的框架是和framework一使用的，这里需要把对应的.m文件删除
 2. 框2 同上 如果有单独的系统库 也是一样的处理方式，如果framework需要使用的没有添加进来可以加进来
 3. 框3 这里的文件都是会在framework的Headers里面显示出来的文件，头文件必须引用必须都是能看得到的状况下引用，没有的添加到系统的PCH文件里面
 4. 框4 参考注意事项第1点，把封装好的接口文件拖入到框3即可，保证framework和项目文件的不重复

![这里写图片描述](https://img-blog.csdn.net/20180627144702678?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzIzOTA3NDY3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

接下来在编译之前,如果你原来的工程中包含有PCH文件,这时你需要在静态库工程中重新修改下它的路径，再添加一次即可。

## 第五步
	所有工作准备完成，可以选择真机或者是模拟器进行编译即可。
	项目中Products文件下的framework直接Show in finder 打开就是你生成的framework
	图片，xib等资源文件都放入了Resource.bundle里面
	Headers文件下即是开放出去的类文件
![这里写图片描述](https://img-blog.csdn.net/20180627145656352?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzIzOTA3NDY3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

最后,就是新建工程,把编译出来的framework拖进去进行测试即可
