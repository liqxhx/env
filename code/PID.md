
## 获取PID
ref:org.springframework.boot.ApplicationPid#getPid
```
  String jvmName = ManagementFactory.getRuntimeMXBean().getName();
  System.out.println(jvmName.split("@")[0]);
```
