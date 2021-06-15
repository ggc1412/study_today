##### synchronized

```java
public class MyHero { 
    private String mHero; 
    
    public static void main(String[] agrs) { 
        MyHero tmain = new MyHero(); 
        System.out.println("Test start!"); 
        
        new Thread(() -> { 
            for (int i = 0; i<1000000; i++) {tmain.batman();} 
        }).start(); 
       
        new Thread(() -> { for (int i = 0; i<1000000; i++) {tmain.superman();} 
                         }).start(); 
        System.out.println("Test end!"); 
    } 
    
    public synchronized void batman() { 
        mHero= "batman";

        try { 
            long sleep = (long) (Math.random()*100); 
            Thread.sleep(sleep); 
            if ("batman".equals(mHero) == false) { 
                System.out.println("synchronization broken"); 
            } 
        } catch (InterruptedException e) { 
            e.printStackTrace(); 
        } 
    } 
    
    public synchronized void superman() { 
        mHero = "superman";

        try { 
            long sleep = (long) (Math.random()*100); 
            Thread.sleep(sleep); 
            if ("superman".equals(mHero) == false) { 
                System.out.println("synchronization broken"); 
            } 
        } catch (InterruptedException e) { 
            e.printStackTrace(); 
        } 
    } 
}
```

결과는 예상 되시나요?

**위에서 synchronized 함수는 객체에 lock을 건다고 얘기했습니다.**

따라서 해당 로그는 절대 찍히지 않습니다.

두개의 thread가 각기 다른 함수를 synchronized 함수를 호출하지만 객체에 lock이 걸리기 때문에 동시에 호출할 수가 없는거죠.



정리하자면 synchronized 함수는 자신이 포함된 객체에 lock을 겁니다.

따라서 동기화 문제를 해결하는데 가장 간단하고 확실하면서 무식한 방법입니다.

여기서 무식하다 함은 synchronized로 인하여 객체에 포함된 다른 모든 synchronized의 접근 까지 lock이 걸리기 때문입니다.



##### sychronized block

```java
Public class SyncBlock2 { 
    private HashMap<String, String> mMap1 = new HashMap<>(); 
    private HashMap<String, String> mMap2 = new HashMap<>(); 
    
    private final Object object1 = new Object(); 
    private final Object object2 = new Object(); 
    
    public static void main(String[] agrs) { 
        SyncBlock2 syncblock2 = new SyncBlock2(); 
        System.out.println("Test start!"); 
        
        new Thread(() -> { 
            for (int i = 0; i<10000; i++) { 
                syncblock2.put1("A","B"); 
                syncblock2.get2("C"); 
            } 
        }).start(); 
        
        new Thread(() -> { 
            for (int i = 0; i<10000; i++) { 
                syncblock2.put2("C","D"); 
                syncblock2.get1("A"); 
            } 
        }).start(); 
        System.out.println("Test end!"); 
    } 
    
    public void put1(String key, String value) { 
        synchronized(object1) { 
            mMap1.put(key, value); 
        } 
    } 
    
    public String get1(String key) { 
        synchronized(object1) {
            return mMap1.get(key); 
        } 
    } 
    
    public void put2(String key, String value) { 
        synchronized(object2) { 
            mMap2.put(key, value); 
        } 
    } 
    
    public String get2(String key) { 
        synchronized(object2) { 
            return mMap2.get(key); 
        } 
    } 
}
```

**이 경우 네개의 메서드 안의 synchronized(this) 블럭으로 감싸진 부분은 동시에 불리지 않습니다.**

**어떤 쓰레드든지 synchronized(this) block에 들어가는 순간 자원을 선점하고 lock을 걸어 둡니다.**

**단 lock의 주체가 this이기 때문에 this로 걸려있는 동기화 block은 해당 lock이 풀릴때까지 대기해야 합니다.**

하지만 lock이 필요한건 같은 hashmap을 동시에 접근하는 경우라고 한다면 put1()과 get1(), put2()와 get2()이 각각 lock을 사용하면 좋을것 같습니다.

>  https://tourspace.tistory.com/55?category=788398