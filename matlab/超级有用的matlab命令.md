# 不定期更新matlab中超级有用的各种小命令

推荐看知乎问题[有哪些让人相见恨晚的 MATLAB 命令？](https://www.zhihu.com/question/24499729)这里只总结我已经亲自体会到对我们的工作有很大帮助的命令。

### 1. [dbstop if error](https://ww2.mathworks.cn/help/matlab/ref/dbstop.html)
对于喜欢写function的我来说，debug是一个超级麻烦的事情。通常而言，如果matlab遇到错误就会直接退出，并且释放function内的所有变量，导致没法检查究竟错在哪里。当然，标准的操作就是设置断点一步一步的检查。但是，在命令行中敲了`dbstop if error`之后，matlab在运行到错误的时候就会自动停在报错的地方，并不释放function中的变量。以后遇到以来之外的错误也可以直接分析了。

dbstop命令本身的作用是*设置断点用于调试*，还有很多参数可以研究，见[matlab的文档](https://ww2.mathworks.cn/help/matlab/ref/dbstop.html)。

### 2. [diary](https://ww2.mathworks.cn/help/matlab/ref/diary.html)
在服务器上提交作业，如果遇到错误有时候输出的报错并不清晰，很难了解到错在哪里。可以使用，matlab会把程序和相应的输出写在diary中。

### 3. [findpeaks](https://www.mathworks.com/help/signal/ref/findpeaks.html)
在matlab里面手动找峰值及其位置虽然也不那么困难，不过这个命令可以自动的批量找出峰来。常用的调用是：
``` matlab
[pks,locs] = findpeaks(data)
```
其中：pks是峰值，locs是峰所走的坐标。
也可以返回更多的值：`[pks,locs,w,p] = findpeaks(data)`。这里w返回峰的宽度，p返回“ the prominences of the peaks”。