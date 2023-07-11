# Project4

## SM3优化方向

### 方向一：消息扩展的快速实现

#### 优化原理

优化前：计算前16个wi时，每个需要执行一次load和一个store，计算后52个wi时，每个需要执行5次load，1次store，6次XOR和1次rot；
计算64个wi，每个wi需要执行2次load，1次store和1次rot。

优化后：
在计算前 12 个 wi+4 时，每个都需要执行一次加载和一次保存。 在计算最后52个wi+4时，每个需要进行5次load，1次store，6次XOR，4次rot；
主要是减少计算和存储W'时的存取操作。 在测试中，优化也提高了算法的执行速度

![image](https://user-images.githubusercontent.com/107350922/180923528-7aef5f25-21ea-4b5a-bcc8-01a00b1888c7.png)

### 方向二：预计算常数

#### 优化原理

常数是预先计算和存储的。 这样可以避免对每个消息包进行不断的移位操作，优化后占用的存储空间也很小，只有256字节。

![image](https://user-images.githubusercontent.com/107350922/180923861-5df4640b-e711-4acf-a80f-73c88cc7d374.png)

### 方向三：优化压缩函数中间变量的生成过程

#### 优化原理

去除了不必要的赋值，减少了中间变量的数量。 方法3、4优化后，只更新了D、h、B、F，减少了赋值操作。压缩函数的结构进行了调整，大大减少了load和store的数量，而中间变量TT1和TT2的优化进一步减少了rot的数量。

![image](https://user-images.githubusercontent.com/107350922/180924370-1f099156-90bc-4f06-838f-f9034d35cc0a.png)

### 方向四：对压缩函数进行结构性调整

#### 优化原理

在每一轮压缩函数结束时，将执行一次循环右移。 将string循环向右移动可以改变每轮输入string的顺序，这个顺序变化会在4轮后恢复，代码实现如下

![image](https://user-images.githubusercontent.com/107350922/180924074-ebd632ef-3ca1-471c-a32c-3e194577eecd.png)


## 结果展示

![image](https://github.com/1-14/Project4/blob/main/%E6%88%AA%E5%9B%BE/1.png)

## 效率比较

如下图所示，在加入上面的优化方法后，对比初始实现方案与openssl中的sm3库的运行效率，算法的运行效率得到了提高，但仍然与标准库函数的运行效率有一定的差距

![image](https://github.com/1-14/Project4/blob/main/%E6%88%AA%E5%9B%BE/2.png)














