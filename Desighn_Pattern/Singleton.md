# 기본 개념
Class에 `Instance가 1개`만 있도록 하여 해당 Instance에 `전역 접근`
# 문제

1. Class에 Instance가 1개만 있도록함
	- 일반적으로 공유 리소스에 대한 접근을 제어하기 위해 사용
		- E.g) DB , File 접근
	- 생성한 `객체를 다시 생성` 시 `기존의 객체를 반환`
2. 해당 Instance에 대한 전역 접근 지점을 제공
	- 전역 변수를 사용하면 여러 곳에서 접근
		- Context Change시 값 충돌의 가능성이 있다.
	- 1. 번의 해결책을 이용하여 Instance를 덮어쓰지 못하도록 보호한다.

# 사용 상황
1. 단일 Instance만 존재해야할 떄 사용
	- E.g) DB , File 접근

# 장단점
| 장점                                 | 단점                                                  |
| ------------------------------------ | ----------------------------------------------------- |
| class가 1개의 Instacne를 갖는다      | 단일 책임 원칙을 위반한다                             |
| Instance에 대한 전역 접근이 가능하다 | Dependancy가 너무 강해질 수 있다                      |
| 최초 1회만 초기화된다                | Multi-Thread상황에 여러번 생성될 수 있어 조심해야한다 |


# 예시 코드

``` java
//Single Thread
//Multi-Thread 에서 동작시 문제가 발생할 가능성이 높다.
public final class Singleton {
    private static Singleton instance;
    public String value;

    private Singleton(String value) {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException ex) {
            ex.printStackTrace();
        }
        this.value = value;
    }

    public static Singleton getInstance(String value) {
        if (instance == null) {
            instance = new Singleton(value);
        }
        return instance;
    }
}

//Multi-Thread DCL(Double Checked Locking)
public final class Singleton {
	// volatile : Main Memory에 적재하겠다는 의미
	private static volatile Singleton instance;
	
	public String value;
	private Singleton(String value) {
		this.value = value;
	}
	public static Singletone GetInstance(String value) {
		Singleton result = instance;
		// instance은 Main Memory에 적재되어있기 때문에 1번이라도 생성되어 있으면 값이 적재됨
		// 그럼으로 1차 검증 후 반환
		if (result != null){
			return result;
		}
		// 혹시나 1차 검증에서 검증 되지 않을 수 있으니 동기화 후 2차 검증
		synchronized(Singleton.class){
			if(instance == null){
				instance = new Singleton(value)
			}
			return instance;
		}
	}
}
```

The approach taken here is called double-checked locking (DCL). It
exists to prevent race condition between multiple threads that may
attempt to get singleton instance at the same time, creating separate
instances as a result.

It may seem that having the `result` variable here is completely
pointless. There is, however, a very important caveat when
implementing double-checked locking in Java, which is solved by
introducing this local variable.

You can read more info DCL issues in Java here:
https://refactoring.guru/java-dcl-issue