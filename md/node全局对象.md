# 全局对象
##  global
-   为全局对象（Global Object），它及其所有属性都可以在程序的任何地方访问，即全局变量。
-   直接 a=1 ,a这个属性会挂在 global对象上
-   在nodejs中你不可能在最外层定义变量，因为所有的用户代码都是属于当前模块的，而当前模块本身也不是最外层
-   永远使用var 定义变量，而避免引入全局变量，防止污染命名空间，防止代码的耦合风险
## global上的属性
### process  
-   描述当前进程状态，提供了一个与操作系统的简单接口

-   stdout 标准输出流
-   stdin  标准输入流
-   argv   一个由命令脚本执行时的各个参数组成的数组，第一个参数是node，第二个参数是脚本文件名，之后的是传进来的脚本参数
-   version node的版本号
-   versions    node的版本和依赖
-   pid 当前进程的进程号
-   nextTick(callback)  当前事件环循环结束，调用回调
-   kill(pid,signal)   发送信号给进程
-   memoryUsage()   返回一个对象，描述了node进程所用的内存状况，字节
-   abort() 会让node 退出并生成一个核心文件
-   chdir(directory)  改变当前进程的工作目录，如果失败抛出异常
-   cwd() 返回当前进程的工作目录  会随chdir改变而改变

### Buffer
-   buffer对象。也是构造函数
####   创建buffer
-   var buffer=new Buffer(size)  
-   var buf = new Buffer([10, 20, 30, 40, 50]);
-   var buf = new Buffer("sunyongjian", "utf-8");
-   在utf-8的编码  一个英文字符--》一个字节  一个中文等于三个字节
-   buffer定义每个字节都是16进制的   因为一个字节是八位2进制的组成，2的八次方-1  一个字节转化成10进制最大为255.转化成16进制最大为ff
####   buffer实例的方法

-原型上的方法

- fill  实例.fill()  第一个参数是value (支持number，string)  
  第二个参数是 起始位置   第三个参数是 结束位置   如果给的位置不够会默认截取 value的开始部分 填进 buffer
  如果后面的参数不给，就默认全部替换成value


```
var buffer=new Buffer('珠峰'); 
console.log(buffer);    //<Buffer e7 8f a0 e5 b3 b0>
buffer.fill("A",1,2);
console.log(buffer);    //<Buffer e7 41 a0 e5 b3 b0>
```

- write
> 1 string要写入的内容 
> 2offset偏移量
> 3写的长度
> 4encoding编码

```
var buffer=new Buffer(6);//<Buffer 90 db 0d 00 00 00>
buffer.write("珠",1,3,"utf8");//<Buffer 00 e7 8f a0 00 00>
console.log(buffer);
```


- copy

```
var buff = new Buffer(12);  //先创建一个长度为12的随机取值的buffer
var buff1= new Buffer("一二");   
var buff2= new Buffer("三四"); //把这两个buff copy进去
//buff1.copy（targetBuffer，targetStart，sourceStart，sourceEnd）//目标buffer  目标buffer的开始位置  buff1的截取起始，结束  都是索引包前不包后
buff1.copy(buff,0,0,6);  // 起始位置不写默认是从0 开始    源不写默认从0-结束   
buff2.copy(buff,6,0,6);
```
现在buff1 和buff2 的内容就拼接到 buff里了

- slice  跟数组和字符串的slice 类似
- 返回一个新的buffer 把截取结果给它,不同之处在于newbuffer 修改内容，老buffer的内容也会变

```
var buffer=new Buffer('一二');
buffer.slice(0,7).toString();   // "一二"

```


-构造函数上的方法

- concat  这是构造函数上的方法，里面的参数是一个数组，数组里包含要拼接的buffer  返回一个拼接好的新buffer

```
var buff1=new Buffer([1,2,3]);
var buff2=new Buffer([4,5,6]);
console.log(Buffer.concat([buff1,buff2])); // 
```

- 引入内置模块解决 截取中文 乱码问题
- ​
```
var StringDecoder=require("string_decoder").StringDecoder;
var sd=new StringDecoder;
var buffer=new Buffer("珠峰");
console.log(buffer.slice(0,4).toString()); // 珠�
console.log(buffer.slice(4).toString());   //��
console.log(sd.write(buffer.slice(0,4)));  //珠
console.log(sd.write(buffer.slice(4)));    //峰
```




## 每个当前模块自带而注入的参数

### __dirname
-   表示当前执行脚本所在的目录
### __filename
-   表示当前正在执行的脚本的文件名。它将输出文件所在位置的绝对路径。如果在模块中，返回的值是模块文件的路径。
-   ​

## 内置模块

### util
    util.inspect
