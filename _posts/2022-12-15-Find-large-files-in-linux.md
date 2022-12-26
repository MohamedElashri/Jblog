---
layout: post
published: true
title: "Find large files in linux"
description: "How to find large files in linux using find command ."
date: 2022-12-15
categories: [linux]
keywords: "linux, find, large files, ubuntu"
comments: true

---

Finding large files in Linux can be useful for a variety of reasons, such as identifying files that are taking up a lot of space on your hard drive or looking for files that may contain unnecessary or duplicate information. Here is a tutorial on how to find large files in Linux using the command line:


1. Open a terminal window. You can do this by pressing `Ctrl+Alt+T` or by clicking on the terminal icon in your taskbar or application launcher.
2. Change to the directory where you want to search for large files. For example, if you want to search for large files in your home directory, you can enter the following command:

```
cd ~

```

3. Use the `find` command to search for large files. The find command allows you to search for files based on various criteria, such as the file name, size, and type. To search for large files, you can use the `-size` option followed by a `+` sign and the size of the files you want to find. For example, to find files that are larger than 100MB in the current directory, you can use the following command:

```
find . -size +100M
```

This will search for all files in the current directory and its subdirectories that are larger than 100MB and display the names and paths of those files.


4. You can also use the `du` command to find the sizes of directories and subdirectories. The `du` command displays the sizes of directories and files in a directory tree, starting from the current directory. To find the sizes of directories and subdirectories in the current directory, you can use the following command:

```
du -sh *
```

This will display the sizes of the directories and subdirectories in the current directory, along with their names.

5. To sort the output of the `find` or `du` command in descending order by size, you can use the sort command. For example, to sort the output of the `find` command in descending order by size, you can use the following command:

```
find . -size +100M | sort -nr
```

This will display the names and sizes of the large files in the current directory and its subdirectories, sorted in descending order by size.


6. To save the output of the `find` or `du` command to a file, you can use the `>` operator followed by the file name. For example, to save the output of the `find` command to a file called `large_files.txt`, you can use the following command:

```
find . -size +100M > large_files.txt
```

This will create a new file called large_files.txt in the current directory and save the output of the find command to it. 


If you want to find files in your all filesystem, but not external devices or network file systems you can use the following command:

```
sudo find / -xdev -type f -size +100M
```

The `-xdev` option tells find to skip directories that are on different file systems. This can be useful if you want to search only the current file system and not external devices or network file systems. The `-type f` option tells find to search only for files, not directories.



I hope this tutorial helps you find large files in Linux! Let me know if you have any questions.


