这个是Florian教我的，利用bash的for命令来实现。感谢Florian！以下是他的邮件内容：

> for f in \*.qs; do qsub \$f; done  
> for d in \*; do for f in \$d/\*; do qsub \$f; done; done  
> for f in \*/\*; do qsub \$f; done  
> Use "echo qsub \$f" instead of "qsub \$f" for testing.

以上三个方法中的第三个方法应该是最简单有效的方法。第四行是提醒我们，在linux操作中，最好先把会进行的操作显示出来检查一下，然后再做。

我测试了第三种方法之后，比如`for dd in */go.matlab.job; do qsub $dd; done`，会发现确实提交了子文件夹中的所有*go.matlab.job*命令。但是由于当前文件夹不是*go.matlab.job*，所以立马会报错说找不到计算所需的文件。

针对这个问题，我学习了一下bash中for具体是如何操作的[^bashfor]，然后自己写了下面的脚本：

``` bash
for dd in *Tuv   # 扫参数的文件夹以"Tuv"结尾
do
        echo cd $dd;
        echo qsub go.matlab.job

        cd $dd;
        qsub go.matlab.job

        cd ..
done
```

即先cd进相应的文件夹，然后用`qsub`命令提交[^gom]，再cd回来重复下一个循环。

根据Florian的建议，出于在linux系统上避免误操作的安全性考虑，可以第一次计算的时候把第二块注释掉，跑一遍。检查无误后，去掉注释正式再跑一遍。

运行之后的输出如下：
``` bash
./mulgo 

cd 1_3Tuv  
qsub go.matlab.job  
4701882.batch.css.lan  
cd 2_1Tuv  
qsub go.matlab.job  
4701883.batch.css.lan  
cd 3_2Tuv  
qsub go.matlab.job  
4701884.batch.css.lan  

```


[^bashfor]: [https://blog.csdn.net/guodongxiaren/article/details/41911437](https://blog.csdn.net/guodongxiaren/article/details/41911437)
[^gom]: 不能用我alias的命令，如*gom*，会报错不认识这个命令的。