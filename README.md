# Java

#### hashCode equals HashMap

使用关注的成员（color，type）重写hashCode和equals方法

```
import java.util.HashSet;

public class Solution {
    public static class Dog {
        String color;
        String type;

        public Dog(String color, String type) {
            this.color = color;
            this.type = type;
        }

        public boolean equals(Object obj) {
            if (!(obj instanceof Dog))
                return false;
            if (obj == this)
                return true;
            return this.color.equals(((Dog) obj).color) && this.type.equals(((Dog) obj).type);
        }

        @Override
        public int hashCode() {
            return color.hashCode() + type.hashCode();
        }
    }

    public static void main(String[] args) {
        HashSet<Dog> dogSet = new HashSet<Dog>();
        dogSet.add(new Dog("white", "Husky"));
        dogSet.add(new Dog("white", "Corgi"));

        System.out.println("We have " + dogSet.size() + " white dogs!");

        if (dogSet.contains(new Dog("white", "Corgi"))) {
            System.out.println("We have a white dog!");
        } else {
            System.out.println("No white dog!");
        }
        if (dogSet.contains(new Dog("black", "Corgi"))) {
            System.out.println("We have a black dog!");
        } else {
            System.out.println("No black dog!");
        }
    }


}
```

#### 线程的三种创建方式

1. new Thread().start()
2. new Thread(new Runnable()).start()
3. new Thread(new Callable(), "Task name").start()

```

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

public class Solution implements Callable<Integer> {
    // 一百个线程执行一个任务
    public static void main(String[] args) {
        Solution ctt = new Solution();
        FutureTask<Integer> ft = new FutureTask<>(ctt);
        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + "Thread index: " + i);
            if (i == 20) {
                new Thread(ft, "Task schedule: ").start();
            }
        }
        // 阻塞，任务执行结束返回了才会继续执行
        try {
            System.out.println("result: " + ft.get());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }

    }

    @Override
    public Integer call() throws Exception {
        int i = 0;
        for (; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + " " + i);
        }
        return i;
    }

}
```
