# RMI例子

```java
// 定义接口
public HelloService {
    public String hello(String content);
}

// 实现
public HelloServiceImpl extends UnicastRemoteObject implements HelloService{
    public XXXServiceImpl(){
        super();
    }
    
    public String hello(String content){
        return "hello, "+content;
    }
} 

// 服务端
public class Server{
    public static void main(String[] args) {
        try {
            // 生成代理HelloServiceImpl_stub
            // 发布了一个远程对象HelloServiceImpl
            IHelloService helloService=new HelloServiceImpl();

            // 生成代理RegistryImpl_stub
            // 发布注册服务
            LocateRegistry.createRegistry(1099); // rmiregistry

            // 绑定服务
            Naming.rebind("rmi://127.0.0.1/Hello",helloService); //注册中心 key - value
            System.out.println("服务启动成功");
        } catch (RemoteException e) {
            e.printStackTrace();
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }

    }
}

// 客户端
public class Client {

    public static void main(String[] args) throws RemoteException, NotBoundException, MalformedURLException {
        IHelloService helloService=
                (IHelloService)Naming.lookup("rmi://127.0.0.1/Hello");
        // HelloServiceImpl实例(HelloServiceImpl_stub)
        // RegistryImpl_stub
        System.out.println(helloService.sayHello("world!"));
    }
}
```



# 源码分析

## sun.rmi.server.UnicastServerRef#exportObject(java.rmi.Remote, java.lang.Object, boolean) 发布服务

```java
public Remote exportObject(Remote var1, Object var2, boolean var3) throws RemoteException {
        Class var4 = var1.getClass();

        Remote var5;
        try {
            // 创建远程服务(业务)对象的代理
            var5 = Util.createProxy(var4, this.getClientRef(), this.forceStubUse);
        } catch (IllegalArgumentException var7) {
            throw new ExportException("remote object implements illegal remote interface", var7);
        }

        if (var5 instanceof RemoteStub) {
            this.setSkeleton(var1);
        }

    	// Target包装一个暴露在TCPEndpoint上的对象
        Target var6 = new Target(var1, this, var5, this.ref.getObjID(), var3);
        this.ref.exportObject(var6);
        this.hashToMethod_Map = (Map)hashToMethod_Maps.get(var4);
        return var5;
    }


```



## java.rmi.registry.LocateRegistry#createRegistry(int) 注册中心

```java

```

## sun.rmi.transport.tcp.TCPTransport#exportObject

```java

```



## sun.rmi.transport.tcp.TCPTransport#listen

```java
this.server = var1.newServerSocket();
                Thread var3 = (Thread)AccessController.doPrivileged(new NewThreadAction(new TCPTransport.AcceptLoop(this.server), "TCP Accept-" + var2, true)); // 循环处理请求
                var3.start();
```

