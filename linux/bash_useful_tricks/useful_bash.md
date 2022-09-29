# 1. 有用的帖子

### 基本操作

- [shell脚本中如何在字符串中传递变量](https://blog.csdn.net/u010339879/article/details/100066362)

### 文件操作

- [sed修改文件内容，包括显示文件制定行内容](https://blog.csdn.net/weixin_43145427/article/details/123818952)
- 利用bash从文本文件中读取想要的数据
  - [使用正则表达式抽取所需文本 - 琴水玉 - 博客园](https://www.cnblogs.com/lovesqcc/p/12443108.html)
  - [shell提取字符串中的字母，数字和小数 - Philbert - 博客园](https://www.cnblogs.com/liangxuran/p/14578866.html)
  - [还可以更精细的利用cut依次读取每一行中第几个值](https://jamesgosling.blog.csdn.net/article/details/124703381?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-124703381-blog-103009117.pc_relevant_multi_platform_whitelistv3&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-124703381-blog-103009117.pc_relevant_multi_platform_whitelistv3&utm_relevant_index=1)

### 数学相关

- bash生成随机数
  
  - https://blog.csdn.net/llllyh812/article/details/126798770
  
  - [shell怎么产生一个随机数，要求是0到1之间的，小数位数是16位 - Shell-Chinaunix](http://bbs.chinaunix.net/thread-4176958-1-1.html)

- [生成序列，类似matlab中的linspace的方法](https://blog.csdn.net/Golden_Chen/article/details/115375482)

# 2. 我自己的例子

- 利用bash扫不同的核间距，更改.gjf文件中的相应参数，并用Gaussian进行计算，从输出的文件中提取轨道能量和总能量的信息汇集在一个文件中，方面画出能量vs.核间距的曲线。另外，把输出文件重命名后保存到指定位置

```bash
#!/bin/bash
# 2022-09-29

# Make sure that the last row of the .gjf file is something like "R xxx"

# ----- input -----
name='mol'              # the file name is "name.gjf"
arR=$(seq 0.7 0.05 1.8)   # the list of R values
# -----------------

mkdir sv_logs
mkdir params
rm ./params/param_Etot.dat
rm ./params/param_Eorbitals.dat
rm ./params/param_R.dat

echo 'Remember to reshape the data according ot the No. of orbitals' > ./params/param_Eorbitals.dat

for n in ${arR}  # internuclear distance in A
do
    echo '--------- 1. Change gjf for mol n='${n}' -------------'

    echo ${n} >> ./params/param_R.dat

    echo ''
    echo "show the last row"
    sed -n '$p' ${name}.gjf   # show the last row

    echo "replace the last row"
    sed -i '$c R '${n}'' ${name}.gjf  # replace the last row, -i means directly work on the file

    echo "show the last row again"
    sed -n '$p' ${name}.gjf

    echo ''

    echo '--------- 2. run gaussian -------------'
    /opt/software/gaussian09/g09install/G09_Linux_Binary/tar/g09/g09 ${name}.gjf
    echo ''

    echo '--------- 3. extract Etot and Ehomo -------------'
    grep "SCF Done" ./${name}.log | grep -oP "\-\d*\.\d*" >> ./params/param_Etot.dat # -o 只显示匹配部分, 有了-P这里更高级的表达式才有用
    grep "Alpha  occ. eigenvalues" ./${name}.log | grep -oP "\-\d*\.\d*" >> ./params/param_Eorbitals.dat # -o 只显示匹配部分, 有了-P这里更高级的表达式才有用
    echo ''

    echo '--------- 4. copy and save logs -------------'
    cp ${name}.log './sv_logs/mol_R_'${n}'.log'
    echo ''

done # end of for R
```

- 依次从文件`param_R.csv`中读取之前生成好的随机数R和它的序号，修改.gjf文件，用高斯计算，并进一步生成cube文件。cubegen所需格点参数存在文件`npts`中。然后建立相应的文件夹，把生成的文件存入到制定的文件夹中。

```bash
#!/bin/bash
# 2022-09-29

# Make sure that the last row of the .gjf file is something like "R xxx"

# ----- input -----
name='Gau'              # the file name is "name.gjf"
datafile='param_R.csv'
# -----------------

mkdir sv_data

while read line   # read ${datafile} line by line
do

    # extract the two values of the line
    index=$(echo ${line} | cut -d ' ' -f 1) 
    R=$(echo ${line} | cut -d ' ' -f 2)

    echo '----------------------------------------'
    echo '---------- R'${index}'='${R}' ----------'
    echo '----------------------------------------'
    echo ''

    echo '--- 1. Change gjf for mol R='${R}' ---'

    sed -i '$c R '${R}'' ${name}.gjf  # replace the last row, -i means directly work on the file

    echo 'The last row of '${name}.gjf' is:'
    sed -n '$p' ${name}.gjf   # show the last row of the file
    echo ''

    echo '--- 2. run gaussian ---'
    /opt/software/gaussian09/g09install/G09_Linux_Binary/tar/g09/g09 ${name}.gjf
    echo ''

    echo '--- 3. genereate .cube ---'
    /opt/software/gaussian09/g09install/G09_Linux_Binary/tar/g09/formchk Gau.chk
    /opt/software/gaussian09/g09install/G09_Linux_Binary/tar/g09/cubegen 0 mo=homo Gau.fchk Gau.cube -1 h < npts
    echo ''

    echo '--- 4. copy and save files ---'

    dir_name='sv_data/R'${index}/''          # / is included in the dir_name
    mkdir ${dir_name}                        # remember to make the folder first

    echo 'log/gjf/cube files will be moved to '${dir_name}''

    echo ${name}.log
    cp ${name}.log ${dir_name}${name}.log
    cp ${name}.gjf ${dir_name}${name}.gjf
    cp ${name}.cube ${dir_name}${name}.cube

done < ${datafile}
```