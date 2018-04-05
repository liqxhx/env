## 任务类MyThread
```
//        class MyThread extends Thread {
    class MyThread implements Callable<String> {
        int index;
        String seqId;
        Semaphore semaphore;

        public MyThread(int index, String seqId, Semaphore semaphore) {
            this.seqId = seqId;
            this.index = index;
            this.semaphore = semaphore;
        }

        public String call() throws Exception {
            long start = System.nanoTime();

            String ret = null;

            Random random1 = new Random(System.nanoTime());
            // 业务
            try {
                Thread.sleep(Math.abs(random1.nextInt()) % 1000);
            } catch (Exception e) {
                e.printStackTrace();
            }
//            System.out.printf("%d %s<------------------%d %s %d%n", index, seqId, (System.nanoTime() - start) / 1000 / 1000 / 1000, this.getName(), this.getId());
            System.out.printf("%d %s<------------------used(ms):%d %s %d %n", index, seqId, (System.nanoTime() - start) / 1000 / 1000, Thread.currentThread().getName(), Thread.currentThread().getId());

//            semaphore.release();


            return random1.nextBoolean() ? null : seqId;

//            System.out.printf("%d %s<------------------used(ms):%d %n", index, seqId, (System.nanoTime() - start) / 1000 / 1000 );

//            semaphore.release();
//            return ret;
        }


    /*        @Override
            public void run() {
                long start = System.nanoTime();
                Random random1 = new Random(System.nanoTime());

                // 业务
                try {
                    Thread.sleep(Math.abs(random1.nextInt())%10000);
                } catch (Exception e) {
                    e.printStackTrace();
                }


                System.out.printf("%d %s<------------------%d %s %d%n", index, content, (System.nanoTime() - start) / 1000 / 1000 / 1000, this.getName(), this.getId());
            }*/
    }
```

## 测试方法
```
 @Test
    public void tGetTDRiskDetailPT() throws Exception {
        long start = System.nanoTime();
        int concurrency = 20;

        ExecutorService executor = Executors.newFixedThreadPool(concurrency);

//        Semaphore semaphore = new Semaphore(concurrency);

//        ExecutorService executor = new ThreadPoolExecutor(
//                20, // core pool size
//                20, // max pool size
//                100L, TimeUnit.MILLISECONDS, // keep alive
//                new LinkedBlockingQueue<Runnable>(30), // queue
//                new ThreadPoolExecutor.CallerRunsPolicy()); // handler

        List<Future> futureList = new ArrayList<Future>();

        InputStream in = TDRiskServiceImpl.class.getResourceAsStream("seq_id.txt");
        BufferedReader reader = new BufferedReader(new InputStreamReader(in, "utf-8"));

        String line;
        int t = 0;

        while ((line = reader.readLine()) != null) {
            if (line.startsWith("#")) continue;

//            semaphore.acquire();
//            Future future = executor.submit(new MyThread(++t, line, semaphore));
            Future future = executor.submit(new MyThread(++t, line, null));
            futureList.add(future);
        }

        executor.submit(new Thread() {
            public void run() {
                System.out.println("可以被执行");
            }
        });

        // 等待执行完成, 不能再提交任务
        executor.shutdown();
/*        executor.submit(new Thread() {
            public void run() {
                System.out.println("不能被执行"); // java.util.concurrent.RejectedExecutionException
            }
        });*/


        while (!executor.isTerminated()) {
            Thread.sleep(1000);
            System.out.println("...。。。");
            Iterator<Future> it = futureList.iterator();
            while (it.hasNext()) {
                Future f = it.next();
                if (f.isDone() && f.get() == null) {
                    it.remove();
                }
            }

        }



        if (futureList != null && futureList.size() > 0) {
//            PrintWriter writer = new PrintWriter(
//                    new OutputStreamWriter(new FileOutputStream(new File(System.getProperty("user.dir"), "seq_id_fail.txt")), "utf-8"));


            for (Future f : futureList)
                if (f.isDone()) {
                    if (f.get() != null) {

                    System.out.println("return:" + f.get());

//                        writer.println(f.get());

                    }
                }

//            writer.close();
        }
        System.out.println("total:" + t +", size:" + futureList.size() + ",used(second):" + (System.nanoTime() - start));


    }


   
```
