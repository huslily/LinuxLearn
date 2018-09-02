# LinuxLearn
# This file contains command when I learn the linux
linux每日一命令--cut 
--按文件大小排序 显示前100行 显示后五列
ll -Sh|head -n 100|cut -d ' ' -f 5-
 
一、基本语法
cut是一个选取命令，以行为单位，用指定分隔符将行切分为若干字段，选取所需要的字段。
1、语法格式
cut [option] files
option常用参数如下：
 
-d：用来定义分隔符，默认为tab键，一般与-f配合使用（如果分隔符是空格，必须是两个单引号之间确实有一个空格，是一个哦，不是支持多个）
-f：需要选取的字段，根据-d切分的字段集选取，下标从1开始
-s：表示不包括那些不含分隔符的行，用于去掉注释或者标题一类的信息
-c：以字符为单位进行分割，可以选取指定字符
-b：以字节为单位进行分割，可以选取指定字节，这些字节位置将忽略多字节字符边界（比如：汉字），除非同时指定了-n参数
-n：取消分割多字节字符，只能和-b参数配合使用，即如果字符的最后一个字节落在由-b参数列表指定的范围之内，则该字符将被选出，否则，该字符将被排除。
不难看出上面参数中，-f、-c、-b都是用来表示提取指定范围数据的，这个范围的表示方法如下：
 
N：只取第N项
N-：从第N项一直到行尾
N-M：从第N项到第M项（包括M项）
-M：从第一项到第M项（包括M项）
-：从第一项开始到结束的所有项
二、应用实例
1、基本用法
用-d与-f组合选取字段，这里以PATH为例说明之：
 
复制代码 代码示例:#echo $PATH
/usr/kerberos/bin:/usr/local/bin:/bin:/usr/bin:/home/changquan.scq/bin
 
（1）选取第2个路径：
 
复制代码 代码示例:#echo $PATH | cut -d: -f2
/usr/local/bin
 
（2）选取第2个开始后的所有路径：
 
复制代码 代码示例:#echo $PATH | cut -d: -f2-
/usr/local/bin:/bin:/usr/bin:/home/changquan.scq/bin
 
（3）选取第2到第4个路径，包括第4个路径：
 
复制代码 代码示例:#echo $PATH | cut -d: -f2-4
/usr/local/bin:/bin:/usr/bin
 
（4）选取从第1个到第4个路径，包括第4个路径：
 
复制代码 代码示例:#echo $PATH | cut -d: -f-4
/usr/kerberos/bin:/usr/local/bin:/bin:/usr/bin
 
（5）选取从第1个到最后一个路径的所有路径：
 
复制代码 代码示例:#echo $PATH | cut -d: -f-
/usr/kerberos/bin:/usr/local/bin:/bin:/usr/bin:/home/changquan.scq/bin
（6）选取第1个路径和第3个路径：
 
复制代码 代码示例:#echo $PATH | cut -d: -f1,3
/usr/kerberos/bin:/bin
 
（7）选取第1到第3个路径和第5个路径：
 
复制代码 代码示例:#echo $PATH | cut -d: -f1-3,5
/usr/kerberos/bin:/usr/local/bin:/bin:/home/changquan.scq/bin
2、字符定位
这里以who为例说明之：
 
复制代码 代码示例:#who
changquan.scq pts/02014-05-13 16:21 (10.62.50.159)
changquan.scq pts/12014-05-13 17:53 (10.62.50.159)
changquan.scq pts/22014-05-13 18:09 (10.62.50.159)
changquan.scq pts/32014-05-13 18:34 (10.62.50.159)
changquan.scq pts/42014-05-13 18:37 (10.62.50.159)
changquan.scq pts/52014-05-13 19:08 (10.62.50.159)
 
（1）提取每一行的第3个字节：
 
复制代码 代码示例:#who | cut -c3
a
a
a
a
a
 
（2）提取第1个字节开始到第3个字节，包括第3个字节：
 
复制代码 代码示例:#who | cut -c-3
cha
cha
cha
cha    
cha
 
（3）提取第3个字节开始到结束的所有字节，包括第3个字节：
 
复制代码 代码示例:#who | cut -c3-
angquan.scq pts/02014-05-13 16:21 (10.62.50.159)
angquan.scq pts/12014-05-13 17:53 (10.62.50.159)
angquan.scq pts/22014-05-13 18:09 (10.62.50.159)
angquan.scq pts/32014-05-13 18:34 (10.62.50.159)
angquan.scq pts/42014-05-13 18:37 (10.62.50.159)
angquan.scq pts/52014-05-13 19:08 (10.62.50.159)
 
（4）提取整行，第3个字节不会重叠：
 
复制代码 代码示例:#who | cut -c-3,3-
changquan.scq pts/02014-05-13 16:21 (10.62.50.159)
changquan.scq pts/12014-05-13 17:53 (10.62.50.159)
changquan.scq pts/22014-05-13 18:09 (10.62.50.159)
changquan.scq pts/32014-05-13 18:34 (10.62.50.159)
changquan.scq pts/42014-05-13 18:37 (10.62.50.159)
changquan.scq pts/52014-05-13 19:08 (10.62.50.159)
 
（5）提取每一行的第3到第5个字节和第8个字节：
 
复制代码 代码示例:#who | cut -c3-5,8
anga
anga
anga
anga
anga
 
3、字节定位
依然以who为例说明之：
提取每一行的第3到第5个字节和第8个字节：
 
复制代码 代码示例:#who | cut -b3-5,8
anga
anga
anga
anga
anga
 
咋一看-b与-c没什么区别，其实不然，二者在单字节字符（字母）上基本一样，而在多字节字符（汉字）上有很大区别，这里以汉字提取为例说明之：
 
复制代码 代码示例:#vi test.txt
星期一
星期二
星期三
#cut -b3 test.txt
?
?
?
#cut -c3 test.txt
一
二
三
 
从上面的例子不难看出，-c以字符为单位输出正常，而-b以字节为单位输出乱码。
当然-b在遇到多字节字符时也不是无药可救了，还有个参数-n可以配合使用来告诉cut不要将多字节字符拆开：
 
复制代码 代码示例:#cut -nb 2
#cut -nb 1,2,3
星
星
星
