很多时候我们需要一次性更改一系列计算中的某个参数。有两种方法：


#### 1. 直接批量修改相应的文件内容
比如，要把整个文件夹下各个子文件夹中*para_laser.m*文件内的*theta_rot*参数进行替换。具体的这一行的内容为：
``` matlab
theta_rot = 0*pi/180;  % additional rotation angle
```
现要把所有的`theta_rot = 0*pi`改成`theta_rot = 20*pi`。

可以先用`find`找出所有的该文件，然后用`sed -i`来替换相关语句，其中前后两个操作用*xargs*传递参数。具体如下
``` bash
find -name "para_laser.m" | xargs sed -i "s/theta_rot = 0/theta_rot = 20/g"
```
其中 `-i` 指的是在[原文件上更改](https://www.jianshu.com/p/89163e927a2c)。


值得提醒的是，和所有的其他linux操作中一样，为了安全起见，最好先用 `grep`命令把第一步会`find`出什么显示出来，确认都是正确的之后，再执行替换：

``` bash
find -name "para_laser.m" | xargs grep "theta_rot = 0"
```

**如果要替换的字符串中出现特殊符号**
可以使用转义符`\`。比如要把`str_output = '../00_fromCLY/sv_gs_outputs.mat'`换成`str_output = '../../00_fromCLY\/sv_gs_outputs.mat'`
``` bash
find -name "main.m" | xargs sed -i "s/str_output = '..\/00_fromCLY\/sv_gs_outputs.mat'/str_output = '..\/..\/00_fromCLY\/sv_gs_outputs.mat'/g"
```

#### 2. 把修改好的一个文件复制到所有的文件夹中
有时候需要批量修改的内容比较多，可以曲线救国，先更改一个文件。然后把这个更改后正确的文件一口气复制到所有的目标文件夹中。

比如，我有以下这么多文件夹：
> drwxrwxrwx 1 ... ... 4096 Mar  1 13:44 0.1_linear_single_k0/  
> drwxrwxrwx 1 ... ... 4096 Mar  1 13:44 0.2_compare_linear/  
> drwxrwxrwx 1 ... ... 4096 Mar  1 18:56 0_linear_301pts/  
> ...  
> drwxrwxrwx 1 ... ... 4096 Mar  1 18:56 8_1to3_301pts/  
> drwxrwxrwx 1 ... ... 4096 Mar  1 19:13 9_1to4_301pts/

其中以*pts*结尾的文件夹中的*HHS_statistics.m*文件的内容都需要做相同的更改，但是这个更改用上面的方法不容易实现。我可以首先在当前位置改好一个正确的*HHS_statistics.m*，然后把他复制到所有的目录中。

具体的思路仍是先用`find`找到想要的文件夹，（用`grep`）确认`find`的结果无误后，用xargs传参并用cp进行替换：
``` bash
find -type d -name "*pts" | xargs -n 1 cp -r ./HHS_statistics.m
```
这里值得注意的是采用了`-n 1`参数[^n1]，否则会失败。他的含义是 [告诉 xargs 命令每个命令行最多使用一个参数，并发送到 cp 命令中](https://blog.csdn.net/yangshangwei/article/details/54424526)。

