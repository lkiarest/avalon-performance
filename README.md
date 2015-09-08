# avalon-performance
avalon性能问题总结

__基于avalon 1.4.6研究__

在使用avalonjs框架的过程中发现经常会有内存泄漏的情况发生，将最近优化的一些方法总结一下

### 1. 使用mmstate切换页面时候发生的内存泄漏

[测试页面](http://lkiarest.github.io/oniui-extend/#!/duplexDemo)
```
使用mmstate路由，测试页面2基本就是一个空的只显示一个字符串，测试页面1里面放几个input，使用ms-duplex绑定到vm的属性。
在页面1和2之间切换的时候发现内存会一直增长不会释放。
使用chrome调试的话会发现avalon.$$subscribers这个数组里的元素会持续增加
经过司徒大神修改之后的版本解决了直接属性泄漏的问题，不过在使用对象或者数组的时候这个还是会有内存泄漏
```

###### 解决方法：

1. onload的时候重新设置对象(=)和数组(pushArray)的值，**空对象也需要重新设置**
1. unload的时候调数组的clear方法或者置为"[]"，(对象不用清除)
