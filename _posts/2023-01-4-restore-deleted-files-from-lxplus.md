---
post: post
title: "Restore deleted files from lxplus"
categories: [Linux, Server, CERN, lxplus]
keywords: "Linux, Server, CERN, lxplus"
comments: true
---

Earlier today I was trying to connect my VScode to lxplus and I was unable. I was trying to connect to particular working folder but it seems that vscode can't find this folder. This was strange so I opened my terminal and then sshed to lxplus. I was surprised to find that this folder on my work directory is not there. This was annoying as it contains some MC files I generated locally which took somedays (as I still early in my analysis and don't have a GRID ready analysis). The idea of having of having to generate all of them again was not good by any mean. I don't have time for that. Beside in the process of generating them I was very annoying for CERN as I got many warning emails from CERN about my high memory usage (yes Gauss is a RAM monester). The source code for Gauss-job and analysis scripts weren't a problem as I always have them pushed to a git repository on my github account. So I was able to recover them from there. But the MC files were a problem.

Now I was hoping there was a way to restore the deleted files. I went to Twiki and looked for a solution and found an article that describes the process. I was happy for know that all `afs` is being backuped regularly. Also they backup personal `home` and `work` spaces for each user. There is a difference however between both of them as the files stored in `home` and deleted are cached and you don't need to get them from `tape`. Anyway it was good to know that `home` directory is backed each night (CERN time) and cached inside `/afs/cern.ch/ubackup/m/melashri/*` where I could easily find the deleted files (up to 24 hours) beside other files. But actually my MC files were on `work` directory which is not cached. The backup are actually on tape which is slow to restore and process it a little bit more involved than just move the files back. 

To store the files, the first was to create an empty folder with the following 

``` bash
afs_admin recover /afs/cern.ch/work/m/melashri/<path_to_my_missing_files>
```

The output asked me what backup I would like to recover. I chosed the latest one and then it will take sometime (long time) to restore the files. The backup will be written to a temporary folder and then you need to move the files to the correct location. The output will give you the path to the temporary folder. I then moved the files to the correct location. 

This was my first day after the new year holiday and I was feeling horrible that I had this issue. I don't recall what happened last week that could lead to this but anyway I was happy that everything worked out and now I can continue struggling with my analysis.
