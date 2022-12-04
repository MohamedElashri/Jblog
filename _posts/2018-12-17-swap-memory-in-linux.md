---
layout: post
title: "Swap Memory in Linux"
description: "How to create and use swap memory in Linux"
keywords: "linux, swap, memory, ubuntu, debian"
comments: true
---

-----------------------

In Linux, swap memory is a type of virtual memory that is used when the system's physical memory (RAM) is full. It allows the system to temporarily store data in the swap space on the hard drive when the RAM is full, which can help improve system performance.

To create swap memory in Linux, you can use the `fallocate` command to create a file that will be used as swap space. For example, to create a 1 GB swap file, you can use the following command:


``` bash
sudo fallocate -l 1G /swapfile
```

This command creates a file named `/swapfile` with a size of 1 GB (1024 MB) in the root directory.

Next, you need to set the appropriate permissions on the swap file. This can be done using the chmod and chown commands. For example, to set the permissions to 600 (read and write access only for the owner), you can use the following command:

``` bash
sudo chmod 600 /swapfile
```

To set the owner of the swap file to the root user, you can use the following command:

``` bash
sudo chown root:root /swapfile
```

After setting the permissions on the swap file, you can use the `mkswap` command to initialize it as swap space. For example, you can use the following command:

``` bash
sudo mkswap /swapfile
```

Finally, you can enable the swap file by using the `swapon` command. For example, you can use the following command:

``` bash
sudo swapon /swapfile
```

After enabling the swap file, it will be used by the system whenever the RAM is full. You can check the status of the swap space using the free command. For example, the following command shows the total, used, and free swap space:

``` bash
free -h
```

Once you have created a swap file and enabled it in Linux, you can tune the kernel to use it more efficiently. This can help improve system performance by allowing the kernel to make better use of the available swap space.

To tune the kernel to use the swap memory, you can modify the `vm.swappiness` and `vm.vfs_cache_pressure` sysctl parameters. These parameters control how the kernel uses the swap space and the page cache, respectively.

The `vm.swappiness` parameter determines how aggressively the kernel will use the swap space. A value of 0 means that the kernel will only use the swap space when the physical memory is completely full, while a value of 100 means that the kernel will use the swap space even when there is free physical memory available.

The `vm.vfs_cache_pressure` parameter determines how aggressively the kernel will shrink the page cache. A value of 0 means that the kernel will never shrink the page cache, while a value of 100 means that the kernel will aggressively shrink the page cache.

To modify these `sysctl` parameters, you can use the sysctl command. For example, to set the `vm.swappiness` parameter to 10 and the `vm.vfs_cache_pressure` parameter to 50, you can use the following commands:

``` bash
sudo sysctl vm.swappiness=10
sudo sysctl vm.vfs_cache_pressure=50
```

These commands set the `vm.swappiness` and `vm.vfs_cache_pressure `parameters to the specified values. You can use the `sysctl` command to view the current values of these parameters as well.

Note that the sysctl parameters are reset to their default values when the system is restarted. To make the changes permanent, you can add the sysctl commands to the `/etc/sysctl.conf `file. This file is read by the kernel when the system is booted, and the specified `sysctl` parameters will be set automatically.

To add the sysctl commands to the `/etc/sysctl.conf` file, you can use the following commands:

``` bash
echo "vm.swappiness=10" | sudo tee -a /etc/sysctl.conf
echo "vm.vfs_cache_pressure=50" | sudo tee -a /etc/sysctl.conf
```


### update

Recently I have created a [bash script](https://gist.github.com/MohamedElashri/559c35548e5985ad6e96c461156eec07) that automates the process of creating and enabling swap memory in Linux. It is mainly intended for use on on low memory machines. 