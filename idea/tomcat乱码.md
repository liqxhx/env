#

```
1
settings -> File Encodings
Global Encoding/Project Encoding/Default encoding for properties files: UTF-8

2
tomcat启动参数
JAVA_OPTS = -Dfile.encoding=UTF-8
JAVA_TOOL_OPTIONS = -Dfile.encoding=UTF-8
 
3
idea vm参数
Help -> Edit Custom VM Options
-Dfile.encoding=UTF-8


```
