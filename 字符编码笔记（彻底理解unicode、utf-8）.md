## 字符编码笔记（彻底理解unicode、utf-8）

世界上存在着多种编码方式，同一个二进制数字可以被解释成不同的符号。
因此，要想打开一个文本文件，就必须知道它的编码方式，否则用错误的编码方式解读，就会出现乱码。
为什么电子邮件常常出现乱码？就是因为发信人和收信人使用的编码方式不一样。
可以想象，如果有一种编码，将世界上所有的符号都纳入其中。
每一个符号都给予一个独一无二的编码，那么乱码问题就会消失。
这就是 Unicode，就像它的名字都表示的，这是一种所有符号的编码。


常用的ASCII码只规定了128个常见字符的编码。这里面包含了英文世界里的常用字符。
但并不适用于其他语言，比如中文，其包含约10w个字符。

**而unicode 收录了世界上绝大部分字符，提供了一个丰富的、包含了全世界所有常用字符的集合。
但 unicode只提供了字符的二进制代码，并没有声明每一个字符应该如何存储和表示**

这里就有两个严重的问题，第一个问题是，如何才能区别 Unicode 和 ASCII ？计算机怎么知道三个字节表示一个符号，而不是分别表示三个符号呢？第二个问题是，我们已经知道，英文字母只用一个字节表示就够了，如果 Unicode 统一规定，每个符号用三个或四个字节表示，那么每个英文字母前都必然有二到三个字节是0，这对于存储来说是极大的浪费，文本文件的大小会因此大出二三倍，这是无法接受的。

```
Unicode符号范围     |            UTF-8编码方式
(十六进制)          |              （二进制）
----------------------+---------------------------------------------
0000 0000-0000 007F | 0xxxxxxx
0000 0080-0000 07FF | 110xxxxx 10xxxxxx
0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
```

下面，还是以汉字`严`为例，演示如何实现 UTF-8 编码。

`严`的 Unicode 是`4E25`（`100111000100101`），根据上表，可以发现`4E25`处在第三行的范围内（`0000 0800 - 0000 FFFF`），因此`严`的 UTF-8 编码需要三个字节，即格式是`1110xxxx 10xxxxxx 10xxxxxx`。然后，从`严`的最后一个二进制位开始，依次从后向前填入格式中的`x`，多出的位补`0`。这样就得到了，`严`的 UTF-8 编码是`11100100 10111000 10100101`，转换成十六进制就是`E4B8A5`。