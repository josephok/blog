---
layout: post
title: 'Apache2 .htaccess文件详解'
date: 2014-02-13 08:00
comments: true
categories: 
---
[htaccess文件](http://en.wikipedia.org/wiki/Htaccess) 是Apache默认的directory级别的配置文件。

在Apache的主配置文件(http.conf或者是apache2.conf，以下用apache2.conf)里可以设置：

```
# AccessFileName: The name of the file to look for in each directory
# for additional configuration directives.  See also the AllowOverride
# directive.
#

AccessFileName .htaccess
```

可以看到，htaccess可以改为其他名字。

在apache2.conf中可以设置htaccess的覆盖权限：

```
#
# Disable access to the entire file system except for the directories that
# are explicitly allowed later.
#
# This currently breaks the configurations that come with some web application
# Debian packages.
#
#<Directory />
#	AllowOverride None
#	Order Deny,Allow
#	Deny from all
#</Directory>
```

如果将AllowOverride设置为None，则htaccess文件将不起作用；将AllowOverride设置为All，那么htaccess里面的指令将覆盖之前设置的；也可以设置其对某些指令可以覆盖，如`AllowOverride AuthConfig Indexes`指定htaccess只能覆盖这两个指令。

＊指令的作用范围

.htaccess文件中的配置指令作用于.htaccess文件所在的目录及其所有子目录，但是很重要的、需要注意的是，其上级目录也可能会有.htaccess文件，而指令是按查找顺序依次生效的，所以一个特定目录下的.htaccess文件中的指令可能会覆盖其上级目录中的.htaccess文件中的指令，即子目录中的指令会覆盖父目录或者主配置文件中的指令。

* 使用htaccess修改php.ini设置

必须先设置权限："AllowOverride Options" 或者 "AllowOverride All"

然后使用下面的格式来设置：

`php_value setting_name setting_value

例如：`php_value  upload_max_filesize  10M` 

再来看phpinfo(): 

`upload_max_filesize	10M`
