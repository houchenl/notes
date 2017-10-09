
# rxjava2

## 集成
// rxjava2    http://jcenter.bintray.com/io/reactivex/rxjava2/rxjava/
compile "io.reactivex.rxjava2:rxjava:2.x.y"

// rxandroid    http://jcenter.bintray.com/io/reactivex/rxjava2/rxandroid/
compile "io.reactivex.rxjava2:rxandroid:2.x.y"


## 与rxjava不同之处

https://github.com/ReactiveX/RxJava/wiki/What's-different-in-2.0

1. They will have different group ids (io.reactivex.rxjava2 vs io.reactivex) and namespaces (io.reactivex vs rx).
2. RxJava 2.x no longer accepts null values and the following will yield NullPointerException immediately or as a signal to downstream.


## 基础使用

