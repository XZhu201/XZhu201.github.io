有一次遇到了parpool总是失败，没法并行起来运算的问题,比如：
> Error from try: Parallel pool failed to start with the following error. For more detailed information, validate the profile 'local' in the Cluster Profile Manager. 

网上查了各种办法，但是我当时的问题是这样解决的：
1. 找到文件夹：`/home/nhbbzhux/.matlab`
2. 删除（保险起见重命名）文件夹`local_cluster_jobs`