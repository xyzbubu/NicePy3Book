!\[image.png\]\(http://upload-images.jianshu.io/upload\_images/3203841-d2c41b0319a26c49.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240\)







\#\#\#简单的说,GUI编程就是给程序加上图形化界面.

&gt;python的脚本开发简单,有时候只需几行代码就能实现丰富的功能,而且python本身是跨平台的,所以深受程序员的喜爱.



\#\#\#如果给程序加一个图形化界面,那么普通的用户也就能用上python的脚本,极大提升工作效率,所以给python程序加上图形化界面,把自己写的脚本,提供给普通用户,的确是一件激动人心的事!



---





\#\#如何给python脚本加图形化界面?

\#\#\#\#\#作者首先考虑了通过浏览器运行python的图形化界面,为了理想的效果,python需要借助javascript实现一些功能,而且python需要额外安装pyv8模块,我折腾了一下,发现pyv8模块安装很麻烦,而且依赖的库很多,编译安装也根据不同的操作系统,存在各种坑,pyv8不适合普通用户,于是就暂时搁置了pyv8模块.



!\[软件界面\]\(http://upload-images.jianshu.io/upload\_images/3203841-4d125e0b232d8421.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240\)





\#\#\#\#\#随后我又比较了pyqt5与tkinter两个模块:



\#\#\#\#\#pyqt5功能很强,界面也漂亮,但语法比较复杂,pyqt5模块需要单独安装,不适合新手入门;



\#\#\#\#\#tkinter是python3自带的模块,能满足基本的功能需求,语法也简单,基本上5分钟就能上手,所以最终选择了tkinter.





\#\#网上当前的python GUI教程存在的问题:

\#\#\#\#1.功能太简单,基本功能就是"花式"显示"Hello World";



\#\#\#\#2.注释不明了,复制粘贴别人写的博客代码,代码残缺



\#\#\#\#3.版本老旧,都是针对python2.7的程序,导入方式如 \`import Tkinter\`,python3应为\`import tkinter\`



\#\#\#这次作者选择了一个 "根据ip地址定位地理位置"的脚本,作为本次教程的素材,比较好玩,也比较容易实现:













\#\#解释的内容都放到了注释里,上代码:







\`\`\`python

import tkinter

import pygeoip



class FindLocation\(object\):

    def \_\_init\_\_\(self\):

        self.gi = pygeoip.GeoIP\("./GeoLiteCity.dat"\)

        \# 创建主窗口,用于容纳其它组件

        self.root = tkinter.Tk\(\)

        \# 给主窗口设置标题内容

        self.root.title\("全球定位ip位置\(离线版\)"\)

        \# 创建一个输入框,并设置尺寸

        self.ip\_input = tkinter.Entry\(self.root,width=30\)



        \# 创建一个回显列表

        self.display\_info = tkinter.Listbox\(self.root, width=50\)



        \# 创建一个查询结果的按钮

        self.result\_button = tkinter.Button\(self.root, command = self.find\_position, text = "查询"\)



    \# 完成布局

    def gui\_arrang\(self\):

        self.ip\_input.pack\(\)

        self.display\_info.pack\(\)

        self.result\_button.pack\(\)



    \# 根据ip查找地理位置

    def find\_position\(self\):

        \# 获取输入信息

        self.ip\_addr = self.ip\_input.get\(\)

        aim = self.gi.record\_by\_name\(self.ip\_addr\)

        \# 为了避免非法值,导致程序崩溃,有兴趣可以用正则写一下具体的规则,我为了便于新手理解,减少代码量,就直接粗放的过滤了

        try:



            \# 获取目标城市

            city = aim\["city"\]

            \# 获取目标国家

            country = aim\["country\_name"\]

            \# 获取目标地区

            region\_code = aim\["region\_code"\]

            \# 获取目标经度

            longitude = aim\["longitude"\]

            \# 获取目标纬度

            latitude = aim\["latitude"\]

        except:

            pass

        

        \# 创建临时列表

        the\_ip\_info = \["所在纬度:"+str\(latitude\),"所在经度:"+str\(longitude\),"地域代号:"+str\(region\_code\),"所在城市:"+str\(city\), "所在国家或地区:"+str\(country\), "需要查询的ip:"+str\(self.ip\_addr\)\]

        \#清空回显列表可见部分,类似clear命令

        for item in range\(10\):

            self.display\_info.insert\(0,""\)



        \# 为回显列表赋值

        for item in the\_ip\_info:

            self.display\_info.insert\(0,item\)

        \# 这里的返回值,没啥用,就是为了好看

        return the\_ip\_info





def main\(\):

    \# 初始化对象

    FL = FindLocation\(\)

    \# 进行布局

    FL.gui\_arrang\(\)

    \# 主程序执行

    tkinter.mainloop\(\)

    pass





if \_\_name\_\_ == "\_\_main\_\_":

    main\(\)

    



\`\`\`

运行效果\(为了更好的演示效果,使用了gif图,图片尺寸较大,建议在wifi环境下观看,土豪随意~\):









!\[一张很有尺寸的演示图!\]\(http://upload-images.jianshu.io/upload\_images/3203841-53e9b6e7b63c6de0.gif?imageMogr2/auto-orient/strip\)









&gt;由于离线查询ip需要全球IP的分布数据，所以我直接选择了一个免费离线查询ip的数据包,为了读取这个包的数据还需要安装一个模块：\`pip install pygeoip\`,极少数人的当年安装python3的时候,选了不含tkinter的python3安装包,为了学习,还是要把这个模块补上:\`pip install tkinter\`



--- 9月27日更新----

&gt; 如果想将示例程序转为windows下的可执行文件\(.exe\),参考这篇http://www.jianshu.com/p/64cb9108a7c6



&gt;教程涉及到的资源我都通过百度网盘分享给大家,为了便于大家的下载,资源整合到了一张独立的帖子里,链接如下:

&gt;

&gt;http://www.jianshu.com/p/4f28e1ae08b1
