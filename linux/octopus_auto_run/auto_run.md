这是一个自动在gs和td之间切换的脚本。后面还直接调用matlab处理数据了。

- 其中的关键仍然是用好sed
- 其中我发现``sed -i``的使用中，最后需要加一个``/g``的能生效。
- 里面多次提到到了``3p``，是因为我的``CalculationMode``语句在第三行。以后也可以用``sed -i`` 通过查找替换的方式来实现，也许更稳定一些。至少行数发生了变化也无所谓。

```bash
## ------ auto run ------

## ## generate E(t)
## echo "Generate E(t)"
## nohup $matlab_path<Output_laser_A.m 1>laser.out 2>laser.err

## check old CalculationMode
echo "Old 3rd row is:"
sed -n '3p' ./inp

## fix to gs
sed -i '3c CalculationMode = gs' ./inp

echo "Fix the 3rd row to:"
sed -n '3p' ./inp

$mpirun_path -np $hostnums -hostfile $hostfile $octopus_path<inp 1>gs.out 2>gs.err

## change gs to td
sed -i 's/CalculationMode = gs/CalculationMode = td/g' ./inp

echo "After gs, change 3rd row to:"
sed -n '3p' ./inp

$mpirun_path -np $hostnums -hostfile $hostfile $octopus_path<inp 1>td.out 2>td.err

## read results with matlab
echo "Read HHG"
nohup $matlab_path<D1_ReadCurrent_Octopus.m 1>ReadHHG.out 2>ReadHHG.err

echo "Read density movie"
nohup $matlab_path<read_density_movie_1D.m 1>ReadDenMovie.out 2>ReadDenMovie.err
```

这是一个自动在gs和unocc之间切换的脚本：

```bash
## ------ auto run ------

## check old CalculationMode
echo "Old 3rd row is:"
sed -n '3p' ./inp

## fix to gs
sed -i '3c CalculationMode = gs' ./inp

echo "Fix the 3rd row to:"
sed -n '3p' ./inp

$mpirun_path -np $hostnums -hostfile $hostfile $octopus_path<inp 1>gs.out 2>gs.err

## change gs to unocc
sed -i 's/CalculationMode = gs/CalculationMode = unocc/g' ./inp

echo "After gs, change 3rd row to:"
sed -n '3p' ./inp

$mpirun_path -np $hostnums -hostfile $hostfile $octopus_path<inp 1>unocc.out 2>unocc.err

## read results with matlab
echo "Read band"
nohup $matlab_path<D1_Readband_Octopus.m 1>Readband.out 2>Readband.err

echo "Read potential"
nohup $matlab_path<D1_Readpotential_Octopus.m 1>Readpot.out 2>Readpot.err

#sh gs_td2.sh 1>zzz.out 2>zzz.err

#mpirun -np octopus<inp
```
