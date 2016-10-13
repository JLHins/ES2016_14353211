# DOL 
## Lab2: DOL 实例分析 & 编程

------

### 实验任务

> * 分析example1、example2代码
> * 通过分析xml代码了解dot图的构造方式
> * 使用dol的方式完成简单编程




### [1.Example代码构成](https://www.zybuluo.com/cmd/)
![Exampledir](https://raw.githubusercontent.com/JLHins/src/master/example.png)
    上图是Example1、2的代码组成，其中global.h声明了一些全局变量，剩余的generator,square,consumer分别对应着数据源、数据处理、输出数据三部分。

#### 1.1 generator.c
```c
void generator_init(DOLProcess *p) {
p->local->index = 0;
p->local->len = LENGTH;
}
int generator_fire(DOLProcess *p) {
if (p->local->index < p->local->len) {
float x = (float)p->local->index;
// 将 x 写到 generator 的端口“ PORT_OUT ”上
DOL_write((void*)PORT_OUT, &(x), sizeof(float), p);
p->local->index++;
}
if (p->local->index >= p->local->len) {
DOL_detach(p); // 销毁
return -1;
}
return 0;
}
```
> #####  实验中generator作为数据提供者，初始化后通过调用fire函数不断将数据输出到自身端口PORT_OUT上。

#### 1.2 squre.c
```c
int square_fire(DOLProcess *p) {
    float i;
    if (p->local->index < p->local->len) {
        //  读 square 的端口“ PORT_IN ” , 将值读到 i
         DOL_read((void*)PORT_IN, &i, sizeof(float), p);
         i = i*i; // 做了个平方
        //  写 square 的端口“ PORT_OUT ” , 把 I  写到那个端口
        DOL_write((void*)PORT_OUT, &i, sizeof(float), p);
         p->local->index++;
    }
     if (p->local->index >= p->local->len) {
     DOL_detach(p);
     return -1;
    }
    return 0;
}
```
> #####  Example1中的squre做的是数据处理，通过PORT_IN端口读入数据，处理后输出到PORT_OUT端口。通过Example2我们可以发现在generator与consumer中间的处理是可以级联甚至说并联的，这与现实中的电路十分类似。

#### 1.3 consumer.c
```c
void consumer_init(DOLProcess *p) {
sprintf(p->local->name, “consumer”); // 就是 p->local->name==“consumer”
p->local->index = 0;
p->local->len = LENGTH;
}
int consumer_fire(DOLProcess *p) {
float c;
if (p->local->index < p->local->len) {
DOL_read((void*)PORT_IN, &c, sizeof(float), p);// 读 consumer 的端口“ PORT_IN ”
printf(“%s: %f\n”, p->local->name, c); // 将结构输出到命令行
p->local->index++;
}
if (p->local->index >= p->local->len) {
DOL_detach(p);
return -1;
}
return 0;
}
```
> consumer作为终点通过PORT_IN接收源于generator、经过中间层的处理的数据。

#### 1.4 dot布局文件
##### 在example1完成执行后会在./examples/example1/下出现布局文件example1.xml，里面定义了进程process，通道sw_channel以及连接结点connection。这些定义决定了各个模块间的合作方式。


### [2.实验结果](https://www.zybuluo.com/cmd/)
####example1:
![e1](https://raw.githubusercontent.com/JLHins/src/master/Example1Result.png)
     在squre.c中修改“i = i*i;”为“i=i*i*i”

####example2
![e2](https://raw.githubusercontent.com/JLHins/src/master/Example2Result.png)
    在xml文件中把iterator的值由“3”改为“2”
    
    
DOT截图：
![](https://raw.githubusercontent.com/JLHins/src/master/example2.png)


### [3.实验总结](https://www.zybuluo.com/cmd/)
#####1.通过DOL实例体会DOL编程的特点
#####2.将任务离散化分配到更小的工作单元，再将不同功能的单元通过特定方式连接协同完成工作。这体现的是一种很好的工程思路。