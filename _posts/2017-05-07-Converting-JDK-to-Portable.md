

## Converting JDK Portable



### JDK8 Portable

Follow the below stepts.

For JDK 8u102 things have changed, this worked for me:

1) Download windows JDK exe

2) Open with 7-Zip

3) Dump contents into a directory %JDK-EXE%
	cmd: cd %JDK-EXE%.rsrc\1033\JAVA_CAB10
	cmd: extrac32 111

4) Now have a tools.zip in directory, open it in 7-Zip

5) Extract contents into a new directory %JDK-VERSION%
	cmd: cd %JDK-VERSION%
	cmd: for /r %x in (*.pack) do .\bin\unpack200 -r "%x" "%~dx%~px%~nx.jar"

6) src.zip is in a linux download: jdk-VERSION-linux-x64.tar.

7) Put a copy into %JDK-VERSION%

Now you are ready to go. You might want to setup JAVA_HOME and PATH to point to your %JDK-VERSION% dir and its BIN subdir.


### JDK7 Portable

Follow the below stepts 

1) Create destination folder where you can place the jdk (e.g. C:\jdk8)

2) Download jdk exe from Oracle (e.g. jdk-8u72-windows-x64.exe)

3) Unzip the tools.zip found inside it into the destination folder

4) In cmd.exe, run:
      cd C:\jdk8
      for /r %x in (*.pack) do .\bin\unpack200 -r "%x" "%~dx%~px%~nx.jar"

This solution works for JDK 8 too, without Admin rights.



[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
[frontmatter]: http://jekyllrb.com/docs/frontmatter/
[github-easybook]: https://github.com/laobubu/jekyll-theme-EasyBook
