---
layout: post
title: Linux locale problem
description: fix the locale problem on Linux
tags: linux locale
---


# Linux上locale的问题
有时候在linux使用terminal时会碰上locale的问题，类似于
```shell
$ locale
locale: Cannot set LC_CTYPE to default locale: No such file or directory
locale: Cannot set LC_ALL to default locale: No such file or directory
```
尝试设置时又会有
```shell
locale: Cannot set LC_CTYPE to default locale: No such file or directory
```

Google一下，找到一个合适的解决方案:
```shell
sudo dpkg-reconfigure locales
sudo locale-gen
```

另外
>C = POSIX standards-compliant default locale. Only strict ASCII characters are valid.  
C.utf8 = POSIX standards-compliant locale, extended to allow the basic use of UTF-8. No character upper-lower case relationships and collation orders defined beyond ASCII.  
en_US.utf8 = American English UTF-8 locale. 
It "knows" which non-ASCII Unicode characters are upper/lower case pairs, and sorts them together, upper case immediately before lower case. It also has default sorting orders defined for various non-Latin alphabets.

+linux+ +locale+
