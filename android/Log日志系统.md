
### Logger日志系统的需求
- 线程信息
- 类信息
- 方法信息
- 漂亮打印json、换行、字节流、
- 清空输出
- 点击跳转到源码
- 日志留存：日志打印到屏幕上，同时在SD卡上存一份，崩溃信息网站上留一份。

### 参考资源
- [github-ZhaoKaiQiang-KLog](https://github.com/ZhaoKaiQiang/KLog)
- [github-orhanobut-logger](https://github.com/orhanobut/logger)
- [github-oreofish-logger](https://github.com/oreofish/logger)
- [github-houchen-logger](https://github.com/houchen890213/origin/tree/master/Origin/common/src/main/java/com/yulin/common/logger)
- [如何选择Logger：Android日志系统分析与比较-简书](http://www.jianshu.com/p/316f065cd00c)

### 笔记
易修通
1. 把开发周期分为DEVELOP，DEBUG，BATE，RELEASE，4个阶段，每个阶段对log有不同处理。在当前类中定义一个属性，记录当前阶段，然后在执行log时根据阶段不同，对打印做不同处理。
2. 对i/d/w/e/v，五种log类型，分别创建两个重载方法，一个方法有一个msg参数，另一个方法多一个throwable参数。i类型多一个重载方法，参数为boolean值。
3. 提供一个方法，打印msg到日志文件中。方法有两个参数，一是一个class，从中可以得到simpleName，二是msg。log文件地址在/sdcard/log/System.currentTime.txt。输出的内容包括，当前时间-simplename-msg。

车蚂蚁
4. 把开发周期分为同上4个阶段，static时，根据BuildConfig.DEBUG是否为true，设置当前阶段为DEBUG或RELEASE。4个开发周期中，只有DEBUG时打印log，其它三个时期不打印log。
5. 使用三方Logger打印日志，static代码块中，设置Logger的配置。包括TAG，methodCount，hideThreadInfo，logLevel，methodOffset。
6. 对i/d/w/e/v，五种log类型，重载方法类型同上。不过打印时，使用的是三方的Logger，而不是sdk的Log。
7. 同样提供打印到本地文件的方法，不过在使用自定义的方法打印到本地，同3一样，之外，还调用了三方Logger.wtf打印到本地。

股大师
8. 只提供一个方法打印log。参数一个是tag，一个是msg。根据线程获取信息，再加上tag和msg，使用System.out.println打印出来。
9. 从线程中获取信息，包括，调用处类名、方法名、行数。
10. 只在BuildConfig.DEBUG为true时打印。

综合
1. 分为DEBUG和RELEASE两个阶段，分别处理。DEBUG时打印，RELEASE时不打印。主线程是否不能调用打印到文件的方法。主线程中以IO文件，但最好不要这样做，尤其是IO内容多时。
2. 需要获取当前调用处的类名、方法名、行数，并且点击时自动跳转到代码中。
3. 使用orhanobut的log管理办法，稍做修改。w/e这两种Log，使用orhanobut中的方式，打印多行，其它类型log打印一行，包括线程名-类名-方法名-代码行数-msg。
