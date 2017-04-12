
# aidl

进程1与进程2不能直接通信，只能通过系统底层间接通信。  

进程间通信的四种方式：  
1. activity与activity通过intent通信。  
2. 通过broadcast通信。  
3. 通过contentprovider通信。  
4. 通过service通信。  

### 什么是AIDL
android interface define language.  
使用interface，定义了server和client间通信的桥梁标准。  
适用场景：IPC，多个应用程序，多线程。

### Binder
只有IPC，没有多线程。多个应用程序。

### Messager
只有IPC，没有多线程。

### 基本操作
