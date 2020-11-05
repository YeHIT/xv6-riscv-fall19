# util lab

## 该分支内实现了MIT-6.s081-OS lab util的5个实验具体代码文件位于user文件夹下的sleep.c、pingpong.c、primes.c、find.c、xargs.c

## 各实验注意点如下:




### sleep.c

#### 主要注意点为命令行参数传递,argc为传入的参数个数,该值包括函数名,即如以sleep 1的形式调用则argc为2。 *argv[]为具体各个参数,argv[0]为方法名。
#### tips:可以通过atoi将argv[x]变为数字传入sleep();



### pingpong.c

#### 主要为pipe(管道)的使用,注意管道应从一端写入一端读出,且写后需要关闭才能读出。



### primes.c

####  个人觉得这个实验是这个lab里最难的一个，筛选过程比较容易理解：即每个人(线程)都先打印从上一个人接收到的第一个元素,然后利用这个元素去筛选其他接收到的元素,若无法整除则将其发送给下一个人。

####  我认为这个实验难点在于对管道的理解,管道要求写后读,即只有当前一个人写完后下一个人才能够读取,但这个实验过程存在这种情况:一个人会一边接收前面人的数据,然后判断数据是否符合再发送给另一个人,这存在对管道的重复使用(因为接收和发送都是从同一个管道进行)。如果不加以处理会导致发送者发送的数据最后被自己读取从而陷入死循环。

####  因此我选择引入一个数组用于存放需要发送给后一个人的数据,将这个过程变为:这个人接收前面人的数据,然后判断数据是否符合再写入数组。然后当这个人接收完数据后再将数组内容写入管道。



### find.c

#### 可以参考user目录下ls.c。注意ls的文件规格化处理将文件名后添加了空字符来统一展示,find中应该只加结束符便于判断。另外ls中操作分为文件类型和目录类型,find也是如此操作,对于文件进行判断是否同名;对于目录则获取路径再次调用自身。



### xargs.c

#### 难点在于对这个函数功能的理解。xargs函数目的是通过反复读取命令行的内容,并以该内容为参数不断地执行某个函数。因此我们可以设置两个变量:paramList[]和paramBuf[]来表示参数列表和当前读取到的参数。

#### 注意点:exec()的第一个参数必为调用的函数名,第二个参数为参数列表,且列表最后应为0。

#### 注意点:每次读取的会是一行内容,一行内可以有若干个参数,故需要区分paramBuf[i]应和当前读到内容的下标,并在读取完一个参数后对paramBuf进行重置,注意重置时需要改变地址,避免对同一个地址重复写入造成结果错误。

