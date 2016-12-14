Dalvik ART 对比：

Dalvik和ART是Android平台的JVM，运行dex格式的java程序。
主要区别：

 1.Dalvik使用JIT，ART使用AOT预编译，JIT在程序运行过程中，Dalvik将字节码翻译成机器码执行；ART(Ahead-Of-Time)在程序安装时就已经将字节码翻译成了字节码，因此极大的提高了引用程序的运行效率。快，但是需要更多的Rom空间。
 2. ART在GC效率上的提高，减少线程Pause时间，而且还在内存分配上对大内存的有单独的分配区域，减少碎片内存的产生，避免很多因GC导致的卡顿问题，Google官方的数据，ART的GC相比Dalvik效率提升了2-3倍，内存分配效率提升了10倍 参考 [Android GC 那点事](http://mp.weixin.qq.com/s?__biz=MzI1MTA1MzM2Nw%3D%3D&hmsr=toutiao.io&idx=1&mid=400021278&scene=0&sn=0e971807eb0e9dcc1a81853189a092f3&utm_medium=toutiao.io&utm_source=toutiao.io)
 3. ART对MultiDex的支持，在Dalvik,Dex文件会被dex2opt优化成odex文件，加快运行效率，但是对于multiDex来说 需要使用multiDex库的支持，通过反射插入dexElements的做法，将multiDex-N插入到最后面；而在ART上，dex文件会被dex2oat优化为ELF格式的oat文件,oat文件格式中将多dex压缩在了一起，也就不存在multiDex加载的问题，特别是对于一些热修复框架的preverify的问题，基本在ART上都没有问题。