## 第五章
### 5.1 编译器的安全优化策略
![](https://raw.githubusercontent.com/ABHCYL/Learn-Csapp/main/%E7%AC%AC5%E7%AB%A0/1.png)
当xp==yp的时候,最后的结果等于0。否则xp和yp指向的对象进行了一次交换。由于有两种可能的情况,限制了编译器可能进行的优化.

### 5.4 消除低效的循环
![](https://raw.githubusercontent.com/ABHCYL/Learn-Csapp/main/%E7%AC%AC5%E7%AB%A0/2.png)
把一些在循环中不变的量提前计算出来。由于编译器不能发现一个函数是不是有副作用,所以它假定有副作用,所以不会去主动优化。

### 5.5 减少过程调用
过程调用（即函数调用）开栈要时间。一个普遍的应用是尾递归改迭代。

### 5.6 消除不必要的内存引用（十分鸡肋）
```c
void sum1(int *a, int len, int *ret)
{
    *ret = 0;
    for(int i = 0; i < len; i++)
    *ret += a[i];
}
void sum2(int *a, int len, int *ret)
{
    int s = 0;
    for(int i = 0; i < len; i++)
    s += a[i];
    *ret = s;
}
```
在sum1中，对ret的解引用导致从%edi(ret)%的地址中取值(*ret)赋值给%eax，再从%eax赋值给%edi。循环len次多了 2 *len 条指令，所以sum2更快。

### 5.8 循环展开（十分鸡肋）
下面两个函数功能一样，函数sum1的for步长为3，比sum更快，即所谓循环展开。为什么更快比较复杂不想写。最主要的是（csapp这个优化）太扯淡了。
```c
int sum(int *arr, int len)
{
    int s = 0;
    for (int i = 0; i < len; i++) s += arr[i];
    return s;
}
int sum1(int *arr, int len)
{
    int s = 0;
    for (int i = 0; i < len-2; i+=3)s += arr[i]+arr[i+1]+arr[i+2];
    return s;
}
```

### 5.9 并行性
#### 5.9.1 提高并行性
![](https://raw.githubusercontent.com/ABHCYL/Learn-Csapp/main/%E7%AC%AC5%E7%AB%A0/5.png)

![](https://raw.githubusercontent.com/ABHCYL/Learn-Csapp/main/%E7%AC%AC5%E7%AB%A0/6.png)
乘法器件耗费的周期巨大，又由于其流水特性。所以考虑增加每次循环中的乘法次数，而不必等到乘积结果迭代到下次而产生的延时。
可以猜到有2路并行，必定也能有3路，4路。。。K路（太扯淡了）。

#### 5.9.2 结合变化
![](https://raw.githubusercontent.com/ABHCYL/Learn-Csapp/main/%E7%AC%AC5%E7%AB%A0/7.png)

五个式子哪个最快？

### 5.11 分支预测
简单来说，计算机预测的分支（计算机预测分支是根据函数执行的历史来预测的），预测出错要消耗更多时间。所以避免写预测出错率高的代码（当然这取决于数据如何），或者把if-else 改写为相同功能的其他语句。
### 5.12 内存性能
简单来说，就是理解系统数据的传输以及Cache寄存器的运行机理。 

[从矩阵乘法问题为背景理解内存性能](https://zhuanlan.zhihu.com/p/146250334)

[Csapp第5章的一些分析和批判](https://www.cnblogs.com/chkkch/archive/2011/06/24/2089378.html)
