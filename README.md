[合集 \- blog(2\)](https://github.com)[1\.关于sizeof、strlen的理解与辨析以及strlen的模拟实现10\-24](https://github.com/bz-crab/p/18498642)2\.关于C语言指针类型的总结10\-24收起
# 前言


我个人将目前在C语言中所遇到的指针归类为**8**种，至于为何写第九点，是因为我个人认为第九点极容易与第五点混淆，故总结如下：


# **1\.普通指针**



> 普通指针即最常见的如：int \* 、 char\*等
> 
> 
> 甚至于也可将一个数组如arr\[5]的数组名arr看作是指针类型（因为指针本质上就是地址，而arr 是该数组首元素的地址）
> 
> 
> 但是值得注意的是 sizeof (arr) 与\&arr两种情况所代表的是整个数组的地址，首元素地址和整个数组地址在值上虽然相同，但在各自\+1、\-1操作所跨越的空间极为悬殊，前者\+1只跳过一个元素，后者\+1则是跳过整个数组。


# **2\.野指针**



> 野指针概念：指针指向的位置不可知（随机的、不正确的、没有明确限制的）
> 
> 
> 以下介绍几种野指针情况
> 
> 
> ①指针未初始化
> 
> 
> cpp
> ```
> #include 
> int main()
> {        
>  int *p;//局部变量指针未初始化，默认为随机值
> 
>  *p = 20;
>   return 0;
> }
> ```
> 
> ②指针越界访问
> 
> 
> cpp
> ```
>  #include 
>  int main()
>  {
>    int arr[10] = {0}
>    int *p = &arr[0];
>    int i = 0;
>  for(i=0; i<=11; i++)
>  {
>       //当指针指向的范围超出数组arr的范围时，p就是野指针
>   *(p++) = i;
>   }
>    return 0;
> ```
> 
> ③指针指向的空间释放
> 
> 
> cpp
> ```
>  #include 
>  int* test()
>  {
>  int n = 100;
>  return &n;
>  }
>  int main()
>  {
>  int*p = test();
>  printf("%d\n", *p);
>  return 0;
>  }
> ```
> 
> 出函数的时候变量n申请的空间会归还给操作系统
> 存在p中的地址没有指向的空间


# **3\.无符号型指针（泛型指针）**



> 无符号型指针即void \* 型



> void\*可以用来接收任何类型的地址，这是其优点，但需要注意的是
> 
> 
> void\*类型的指针不能直接进行指针的\+\-整数的运算。
> 
> 
> 注：一般void\*指针使用在函数参数部分，使得一个函数可以处理多种类型的数据，以实现泛型编 程的效果。
> 
> 
> cpp
> ```
> #include 
> int main()
> {
> 	int a = 10;
> 	void* pa = &a;
> 	void* pc = &a;
> 	*pa = 10;
>  *pc = 0;
> 	return 0;
> }
> ```
> 
> 这里我们可以看到， void\* 类型的指针可以接收不同类型的地址，但是无法直接进行指针运算。


# 4\.**二级指针**



> **二级指针概念：存放一级指针地址的指针（本质上还是地址）**


[![](https://i-blog.csdnimg.cn/direct/5bae1b6ecbe148a592835f38e1c12ab2.png)](https://i-blog.csdnimg.cn/direct/5bae1b6ecbe148a592835f38e1c12ab2.png)
[![](https://i-blog.csdnimg.cn/direct/bd855b8482154f0fafef103c3fecd5c6.png)](https://i-blog.csdnimg.cn/direct/bd855b8482154f0fafef103c3fecd5c6.png)

> 至于是跳过4个还是8个字节取决于当前是几位的操作系统。


# **5\.数组指针**



> **数组指针概念：存放的是数组的地址，是能够指向数组的指针变量。**
> 
> 
> [![](https://i-blog.csdnimg.cn/direct/f65e6775e98d4d648dda6e85708869c5.png)](https://i-blog.csdnimg.cn/direct/f65e6775e98d4d648dda6e85708869c5.png)
> 上图p2就是数组指针变量，详细分析可见第8点。
> 
> 
> 数组指针的[初始化](https://github.com)：int(\*p)\[10] \= \&arr;


# **6\.函数指针**



> **函数指针概念：函数指针变量是⽤来存放函数地址的**
> 
> 
> **函数名就是函数的地址，当然也可以通过 \&函数名获得函数的地址，两者等价。**\*\*
> 
> 
> [![](https://i-blog.csdnimg.cn/direct/bb01bd7ac33c480c9a2b187c6b9878ef.png)](https://i-blog.csdnimg.cn/direct/bb01bd7ac33c480c9a2b187c6b9878ef.png)


# **7\.结构体指针**



> csharp
> ```
> struct Test
> { int Num;
> char *pcName;
> short sDate;
> char cha[2];
> short sBa[4];
> }*p = (struct Test*)0x100000;
> ```
> 
> [![](https://i-blog.csdnimg.cn/direct/2956534ec03e4f1e8e3acd36e1732c10.png)](https://i-blog.csdnimg.cn/direct/2956534ec03e4f1e8e3acd36e1732c10.png)
> 如图：P就是结构体指针变量（指向结构体）
> 
> 
> **注：p\+1跳过的是一个结构体的大小。**


# **8\.文件指针**



> **FILE* pf; // 文件指针变量*\*\*
> 
> 
> 定义pf是⼀个指向FILE类型数据的指针变量。可以使pf指向某个文件的文件信息区（是⼀个结构体变 量）。通过该文件信息区中的信息就能够访问该文件。
> 
> 
> 即通过文件指针变量能够间接找到与它关联的文件。
> 
> 
> **至于FILE类型数据解释如下：**
> 
> 
> 每个被使用的文件都在内存中开辟了⼀个相应的文件信息区，用来存放文件的相关信息（如文件的名字，文件状态及文件当前的位置等）。这些信息是保存在⼀个结构体变量中的。该结构体类型是由系统声明的，取名FILE.
> 
> 
> 不同的C编译器的FILE类型包含的内容不完全相同，但是大同小异。 每当打开⼀个文件的时候，系统会根据文件的情况自动创建⼀个FILE结构的变量，并填充其中的信息。
> 
> 
> ⼀般都是通过⼀个FILE的指针来维护这个FILE结构的变量，这样使用起来更加方便。
> 
> 
> [![](https://i-blog.csdnimg.cn/direct/405cd64c65b94b01878e53a776d709e7.png)](https://i-blog.csdnimg.cn/direct/405cd64c65b94b01878e53a776d709e7.png)


# **9\.指针数组（是数组而非指针）**



> 对于指针数组的理解可以参考整型数组和字符型数组
> 
> 
> 如下图
> 
> 
> [![](https://i-blog.csdnimg.cn/direct/b3aa411561354eae95dd6e662165db8f.png)](https://i-blog.csdnimg.cn/direct/b3aa411561354eae95dd6e662165db8f.png)
> [![](https://i-blog.csdnimg.cn/direct/a4d99ce34dd548b7af5f0977e0ad6784.png)](https://i-blog.csdnimg.cn/direct/a4d99ce34dd548b7af5f0977e0ad6784.png)
> ***\*在这里有必要将指针数组与数组指针对比一下\****
> 
> 
> [![](https://i-blog.csdnimg.cn/direct/261395204986435db4afe421eb53689c.png)](https://i-blog.csdnimg.cn/direct/261395204986435db4afe421eb53689c.png)
> 分析：p1优先与\[10]结合，p1是数组名，即1式是指针数组。
> 
> 
> p2优先与\*结合，表明p2是指针变量，即2式是数组指针。


  * [前言](#%E5%89%8D%E8%A8%80):[wgetCloud机场](https://tabijibiyori.org)
* [1\.普通指针](#tid-kB5rsm)
* [2\.野指针](#tid-BRDa8G)
* [3\.无符号型指针（泛型指针）](#tid-fzYcZC)
* [4\.二级指针](#tid-C7zRGX)
* [5\.数组指针](#tid-p7hyTm)
* [6\.函数指针](#tid-CbMnkB)
* [7\.结构体指针](#tid-87R8nT)
* [8\.文件指针](#tid-xfFTcQ)
* [9\.指针数组（是数组而非指针）](#tid-YjPMeK)

   \_\_EOF\_\_

   ![](https://github.com/bz-crab)白藏crab  - **本文链接：** [https://github.com/bz\-crab/p/18501539](https://github.com)
 - **关于博主：** 评论和私信会在第一时间回复。或者[直接私信](https://github.com)我。
 - **版权声明：** 除特殊说明外，转载请注明出处～\[知识共享署名\-相同方式共享 4\.0 国际许可协议]
 - **声援博主：** 如果您觉得文章对您有帮助，可以点击文章右下角**【[推荐](javascript:void(0);)】**一下。
     
