---
title: 札记
date: 2018-11-03 14:59:45
---

## 2018-11-03

No programming language is perfect. There is not even a single best language; there are only languages well suited or perhaps poorly suited for particular purposes.

Shell scripting hearkens back to the classic UNIX philosophy of breaking complex projects into simpler subtasks, of chaining together components and utilities.

>Unix的哲学就是将复杂的任务打破成多个简单的小任务，然后把工作流拼接起来。甚至在设计软件和工具型的程序时也是遵循这种思想，这种做法对程序员来说是一种直觉，这种思想就蕴含在编程中，但普通用户只喜欢傻瓜式操作。而Windows则倾向于迎合普通用户，打造集成环境，尽管许多集成环境都由不少重复的小轮子组成，但我们不缺硬盘空间和内存空间。

运用抽象是走向代码优化的第一步。面向接口编程。面向抽象编程。

消除重复部分。 优秀设计的根本是：消除重复部分！（DRY = Don’t repeat yourself）

回调和多态都是为了解耦，实现更好的模块化，升级代码时候改动更少的代码

我发现我取消Home、catagories、about、tags等原生目录之后，打开博客网站的首页，有个点会停留在第一个目录上，也就是目前的“信息科学”这个目录上。所以我想恢复首页这个目录（之前取消这个目录是因为点击我的博客名，也就是我的网名：liuqinh2s的时候，会跳转到首页，也就是网站根目录，我不想搞两个有相同作用的按钮，但其实我错了，用户并不知道那个可以点击，就像安卓的侧滑出菜单一样，用户并不知道侧滑可以出菜单，所以才需要一个菜单按钮），恢复这个首页目录是设计上的必要。

>设计思维就是要面向用户，不要以为用户知道那些隐含的功能点。

1.色彩的概念：色彩是物体发出或反射的光在视觉系统中的形成的反应，这种反应使人们得以从视觉上区分物体的大小、形状、结构和属性等外部特征。
2.色彩的三特性：亮度Luminance，色调Hue，饱和度Saturation（色调的深浅程度）各种单色光饱和度最高，单色光中掺入白光越多，饱和度越低，白光占绝大部分时，饱和度接近于零，白光的饱和度等于零。注意区分亮度和饱和度。
3.色彩模型：RGB，CMYK（颜料的三原色：C青，M品，Y黄，+K黑，应用于印刷工业）。RGB是加色模型，CMYK是减色模型。HSV（H是色相即色调，S是饱和度（取值0~100%），V是亮度值Value（取值0~100%））。YUV。

和分治算法比较类似，但不同的是分治算法把原问题划归为几个相互独立的子问题，从而一一解决，而动态规划则是针对子问题有重叠的情况的一种解决方案。

## 2018-11-19

看了某人的spring boot代码，发现后台的错误码和错误信息管理的一个优雅的做法：使用一个枚举类封装这两个属性，并使构造函数私有化。代码如下：

```java
public enum ExceptionMsg {
    SUCCESS("000000", "操作成功"),
    FAILED("999999", "操作失败"),
    ParamError("000001", "参数错误！"),

    LoginNameOrPassWordError("000100", "用户名或者密码错误！"),
    EmailUsed("000101", "该邮箱已被注册"),
    UserNameUsed("000102", "该登录名称已存在"),
    EmailNotRegister("000103", "该邮箱地址未注册"),
    LinkOutdated("000104", "该链接已过期，请重新请求"),
    PassWordError("000105", "密码输入错误"),
    UserNameLengthLimit("000106", "用户名长度超限"),
    LoginNameNotExists("000107", "该用户未注册"),
    UserNameSame("000108", "新用户名与原用户名一致"),

    FavoritesNameIsNull("000200", "收藏夹名称不能为空"),
    FavoritesNameUsed("000201", "收藏夹名称已被创建"),

    CollectExist("000300", "该文章已被收藏"),

    FileEmpty("000400", "上传文件为空"),
    LimitPictureSize("000401", "图片大小必须小于2M"),
    LimitPictureType("000402", "图片格式必须为'jpg'、'png'、'jpge'、'gif'、'bmp'");

    private ExceptionMsg(String code, String msg) {
        this.code = code;
        this.msg = msg;
    }

    private String code;
    private String msg;

    public String getCode() {
        return code;
    }

    public String getMsg() {
        return msg;
    }
}
```

## 2018-11-28

一切为了可重用。

代码写得能看懂便于维护，这样代码就能更长久的可重用。

代码模块化，是为了降低耦合，减少依赖，模块内部高内聚，模块之间低耦合，模块可重用。

模块的粒度应该以重用的灵活性为指导原则，不能一味追求小，但基本的客观规律是：比较小的模块更灵活，更容易重用。

一个设计良好的模块的客观标准是：可以随意组合，即插即用。

封装是为了更好的把代码的改动控制在模块内部，从而减少外部的改动需求。API尽量不变，对外的接口尽量不变。[理解 Java 的三大特性之封装](http://wiki.jikexueyuan.com/project/java-enhancement/java-one.html)

继承则是赤裸裸的在重用代码。

## 2019-01-17

一个奇怪的问题，我配置好了github的ssh key，但每次push都跟我索要账号密码。原因是我clone的时候用的http模式，在项目的.git/config里面可以修改，改为ssh模式即可。感谢stackoverflow的小哥：[Why is Github asking for username/password when following the instructions on screen and pushing a new repo?](https://stackoverflow.com/questions/10909221/why-is-github-asking-for-username-password-when-following-the-instructions-on-sc)

## 2019-02-19

今天终于解决了一个困扰我一年之久的问题，网易云音乐和知乎账号的登陆异常。我对比了不同的网络环境（IP），不同的账号（别人的知乎账号），不同的浏览器。组合测试最后发现既不是IP的单方面的问题，也不是账号的单方面问题，也不是浏览器的单方面的问题。而当我使用无痕浏览的时候并没有任何问题，所以应该是chrome浏览器记住了某些东西。最后发现问题出在chrome浏览器的账号同步上，可能是因为我的谷歌账号在历史上曾记录了一次知乎异常登陆的cookie，所以之后的每一次登陆都使用这个cookie，而且清空都是无效的，在你登陆的时候又会给你自动添加。之后我先登出chrome账号，并同时清空所有数据，然后再登陆知乎账号，然后再登陆chrome账号同步一次正确的知乎登陆。问题就得到了解决。

知乎问题的根源：

- https://jiasule.v2ex.com/t/533482
- https://www.v2ex.com/t/521180

答案就是：发现一个在开启 IPFS 伴侣时，知乎的大部分接口都会报请求参数异常的 BUG。。

## 2019-02-20

今天联合查询一个40万和1万的表，发现弄了索引都没啥效果，最后发现是字符集不同导致的。把两个索引的字符集弄成一样的就行了。还有一个问题，mysql8没有缓存导致查询很慢，mysql5.7有缓存第二次查询快的飞起。还有同一个语句mysql8花了100秒，mysql5.7只花了10秒，原因可能是对语句的解释不一样。在将数据库导入到mysql5.7的时候遇到一个错误：`2006: mysql server has gone away`。使用：`set global max_allowed_packet=268435456;`解决了。应该是单笔insert太大导致的，把配置设高一点就OK了。

## 2019-02-27

Java反射，反射的作用包括，在运行时判断一个对象所属的类，给某个未知的类新建一个对象，获取任意一个类的成员变量和方法，并调用。如果你是初学反射，这么说你一定不知道我在说什么。但举个例子就比较容易理解了。那就是用配置文件动态控制程序要加载的类：

举个例子我们的项目底层有时是用mysql，有时用oracle，需要动态地根据实际情况加载驱动类，这个时候反射就有用了，假设 com.java.dbtest.myqlConnection，com.java.dbtest.oracleConnection这两个类我们要用，这时候我们的程序就写得比较动态化，通过Class tc = Class.forName("com.java.dbtest.TestConnection");通过类的全类名让jvm在服务器中找到并加载这个类，而如果是oracle则传入的参数就变成另一个了。这时候就可以看到反射的好处了，这个动态性就体现出java的特性了！举多个例子，大家如果接触过spring，会发现当你配置各种各样的bean时，是以配置文件的形式配置的，你需要用到哪些bean就配哪些，spring容器就会根据你的需求去动态加载，你的程序就能健壮地运行。

在使用Python的时候我就通过用配置来控制代码。体验非常棒。其实反射的作用就是通过配置控制代码。