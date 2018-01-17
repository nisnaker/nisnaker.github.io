---
layout: post
title: 通过ftp命令上传整个目录
date: 2018-01-17 12:54:09
categories:
 - 小技巧
---

分享一个ftp命令上传整个目录的脚本：

```bash
#! /bin/sh

host=ftp_host
user=ftp_user
pass=ftp_pass

remote_path='/'
local_path='/path/to/upload/'

curr_dir=`pwd`

cd $local_path

_mkdir=`find $local_path -type d -printf $remote_path'%P \n'| awk '{if ($0 == "")next;print "mkdir " $0}'` 
_upload=`find $local_path -type f -printf 'put %p %P \n'`


ftp -nv $host << INPUT_END
quote user $user
quote pass $pass
type binary
prompt off
$_mkdir
$_upload
bye
INPUT_END

cd $curr_dir
```
