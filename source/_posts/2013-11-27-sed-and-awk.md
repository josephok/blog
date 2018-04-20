---
layout: post
title: 'sed and awk'
date: 2013-11-27 05:36
comments: true
categories: 
---
```bash
list文件内容：
$ cat list
John Daggett, 341 King Road, Plymouth MA
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Eric Adams, 20 Post Road, Sudbury MA
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston MA

将 MA替换成, Massachusetts: 

$ sed 's/ MA/, Massachusetts/' list
John Daggett, 341 King Road, Plymouth, Massachusetts
Alice Ford, 22 East Broadway, Richmond VA
Orville Thomas, 11345 Oak Bridge Road, Tulsa OK
Terry Kalkas, 402 Lans Road, Beaver Falls PA
Eric Adams, 20 Post Road, Sudbury, Massachusetts
Hubert Sims, 328A Brook Road, Roanoke VA
Amy Wilde, 334 Bayshore Pkwy, Mountain View CA
Sal Carpenter, 73 6th Street, Boston, Massachusetts
```

Sed 参数总结：

|-e |	后接命令|
|-------------|-------------|
|-f	|后接脚本文件名|
|-n	|阻止自动输出|

```bash
{print $1}表示打印第一个字段到标准输出，字段是以空格或者制表符分割的；$1表示一个字段，
$2表示第二个字段，$3表示第三个字段，...以此类推。$0表示所有字段。
$ awk '{print $1}' list
John
Alice
Orville
Terry
Eric
Hubert
Amy
Sal

打印包含MA的行
$ awk '/MA/' list 
John Daggett, 341 King Road, Plymouth MA
Eric Adams, 20 Post Road, Sudbury MA
Sal Carpenter, 73 6th Street, Boston MA

打印包含MA的所有行的第一个字段：
$ awk '/MA/ { print $1 }' list
John
Eric
Sal
```

awk的-F参数可以设定字段分隔符：
```bash
将字段分隔符改为`,`，然后打印第一个字段：
$ awk -F, '/MA/{print $1}' list
John Daggett
Eric Adams
Sal Carpenter
```

awk还可以同时处理多个命令，他们之间有分号`;`隔开：
```bash
打印1，2，3字段
$ awk -F, '{ print $1; print $2; print $3 }' list
John Daggett
 341 King Road
 Plymouth MA
Alice Ford
 22 East Broadway
 Richmond VA
Orville Thomas
 11345 Oak Bridge Road
 Tulsa OK
Terry Kalkas
 402 Lans Road
 Beaver Falls PA
Eric Adams
 20 Post Road
 Sudbury MA
Hubert Sims
 328A Brook Road
 Roanoke VA
Amy Wilde
 334 Bayshore Pkwy
 Mountain View CA
Sal Carpenter
 73 6th Street
 Boston MA
```

awk 参数总结：

|-F |	后接字段分隔符|
|-------------|-------------|
|-f	|后接脚本文件名|
|-v	|后接var=value|

可以使用管道操作将sed与awk联合起来操作文本：
假设sedscript脚本的内容是
```bash
$ cat sedscript
s/ MA/, Massachusetts/
s/ PA/, Pennsylvania/
s/ CA/, California/
s/ VA/, Virginia/
s/ OK/, Oklahoma/
```
那么我们可以使用如下命令来打印州名：
```bash
$ sed -f sedscript list | awk -F ', ' '{print $4}'
Massachusetts
Virginia
Oklahoma
Pennsylvania
Massachusetts
Virginia
California
Massachusetts
```
