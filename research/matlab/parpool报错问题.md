有一次遇到了parpool总是失败，没法并行起来运算的问题,比如：
> Error from try: Parallel pool failed to start with the following error. For more detailed information, validate the profile 'local' in the Cluster Profile Manager. 

网上查了各种办法，但是我当时的问题是这样解决的：
1. 找到文件夹：`/home/username/.matlab`
2. 删除（保险起见重命名）文件夹`local_cluster_jobs`

解决问题的出处不记得了，翻阅浏览器记录，那天我查了这两个带*parpool*的网站：
- [https://ww2.mathworks.cn/matlabcentral/answers/92124-why-am-i-unable-to-use-parpool-with-the-local-scheduler-or-validate-my-local-configuration-of-parall](https://ww2.mathworks.cn/matlabcentral/answers/92124-why-am-i-unable-to-use-parpool-with-the-local-scheduler-or-validate-my-local-configuration-of-parall)

- [https://ww2.mathworks.cn/matlabcentral/answers/141932-matlab-8-2-8-3-parpool-not-working-on-a-cluster](https://ww2.mathworks.cn/matlabcentral/answers/141932-matlab-8-2-8-3-parpool-not-working-on-a-cluster)

