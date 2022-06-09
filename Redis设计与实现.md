# Redis设计与实现

## 第二章 简单动态字符串

+ Redis构建了一种名为简单动态字符串（SDS）的抽象类型，并将SDS用作Redis的默认字符串表示
+ 当Redis需要的不仅是一个字符串字面量，而是一个可以被修改的字符串值时，就会使用SDS来表示字符串值。
+ 比如

~~~shell
redis> RPUSH fruits "apple" "banana"
~~~

+ Redis将在数据库中创建一个新的键值对，其中：
    + 键值对的键是一个字符串对象，底层实现是一个保存了字符串“fruits”的SDS
    + 值是一个列表对象，包含了两个字符串对象。这两个对象分别由两个SDS实现。
+ 除此外，SDS还被用作缓冲区：AOF模块中的AOF缓冲区，以及客户端状态中的输入缓冲区

### 2.1 SDS的定义

![image-20220609213439939](https://github.com/sandubuhan/Redis_design_implementation/blob/main/img/202206092134992.png)

+ free：表示这个SDS未分配的空间
+ len：表示保存的空间
+ buf：是一个char类型的数组
