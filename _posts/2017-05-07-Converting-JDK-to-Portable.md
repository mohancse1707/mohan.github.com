---
layout: post
title: Converting JDK to Portable Unpacking
date: 2017-05-07 00:00:00 Z
keywords: JDK unpacking, JDK8 unpacking, jdk8 portable, jdk portable
tagline: Converting JDK to Portable - Unpacking
---

This blog is created to show how to convert the JDK exe file into portable package. 

### JDK8 Portable

Follow the below stepts.

For JDK 8u102 things have changed, this worked for me:

1) Download windows JDK exe

2) Open with 7-Zip and extract to "jdk-8u261-windows-x64"

3) Open Terminal / Command Prompt and Change Directory to "path/to/jdk-8u261-windows-x64/.rsrc\1033\JAVA_CAB10" then excute this command 

```Example : C:\jdk-8u261-windows-x64\.rsrc\1033\JAVA_CAB10>extrac32 111```

4) Above command generates tools zip folder in the same directory and extract the tools folder using 7-zip

5) Terminal / Command Prompt and Change Directory to "path/to/tools" and execute the following command

```Example C:\jdk-8u261-windows-x64\.rsrc\1033\JAVA_CAB10>for /r %x in (*.pack) do .\bin\unpack200 -r “%x” “%~dx%~px%~nx.jar”```


6) Now your JDK portable is ready to use. copy the tools folder into some other directory and name it as jdk-1.8

7) set the JDK path as JAVA_HOME in environment variable. 

```Example : set JAVA_HOME= C:\jdk-1.8```


This solution works for without Admin rights.



[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
[frontmatter]: http://jekyllrb.com/docs/frontmatter/
[github-easybook]: https://github.com/laobubu/jekyll-theme-EasyBook
