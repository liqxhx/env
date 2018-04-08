
## 获取PID
```
  String jvmName = ManagementFactory.getRuntimeMXBean().getName();
  System.out.println(jvmName.split("@")[0]);
```
