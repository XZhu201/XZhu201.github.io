这个是Florian教我的，利用bash的for命令来实现。感谢Florian！以下是他的邮件内容：

>for f in \*.qs; do qsub \$f; done
for d in \*; do for f in \$d/\*; do qsub \$f; done; done
for f in \*/\*; do qsub \$f; done
Use "echo qsub \$f" instead of "qsub \$f" for testing.
