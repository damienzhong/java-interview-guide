- [1. 什么是java序列化，如何实现java序列化？Serializable接口的作用?](#1-什么是java序列化，如何实现java序列化？Serializable接口的作用?)
- [2. FileInputStream 在使用完以后，不关闭流，想二次使用可以怎么操作？](#2-FileInputStream在使用完以后，不关闭流，想二次使用可以怎么操作)
## 1. 什么是java序列化，如何实现java序列化？Serializable接口的作用?
将一个java对象变成字节流的形式传出去称为序列化，从字节流中恢复一个对象称为反序列化。 实现serializable接口，这样，javac编译时就会进行特殊处理，编译的类才可以被writeObject方法操作，这就是所谓的序列化。需要被序列化的类必须实现Serializable接口，该接口是一个mini接口，其中没有需要实现方法，implements Serializable只是为了标注该对象是可被序列化的。
## 2. FileInputStream在使用完以后，不关闭流，想二次使用可以怎么操作
