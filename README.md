# java-racingcar-kakao

## String Calculator

### 기능 요구사항
* 쉼표(,) 또는 콜론(:)을 구분자로 가지는 문자열을 전달하는 경우 구분자를 기준으로 분리한 각 숫자의 합을 반환 (예: “” => 0, "1,2" => 3, "1,2,3" => 6, “1,2:3” => 6)
* 앞의 기본 구분자(쉼표, 콜론)외에 커스텀 구분자를 지정할 수 있다. 커스텀 구분자는 문자열 앞부분의 “//”와 “\n” 사이에 위치하는 문자를 커스텀 구분자로 사용한다. 예를 들어 “//;\n1;2;3”과 같이 값을 입력할 경우 커스텀 구분자는 세미콜론(;)이며, 결과 값은 6이 반환되어야 한다.
* 문자열 계산기에 숫자 이외의 값 또는 음수를 전달하는 경우 RuntimeException 예외를 throw한다.

### 사용법
#### StringCalculator
##### String[] split(String string)
* 기본적으로 `,`와 `:`를 기준으로 분할
* `//{custom delimiter}\n{string}` 형식으로 입력시 기본 구분자를 대신해 `custom delimiter`를 기준으로 분할
```java
class Application {
    static void main() {
      StringCalculator stringCalculator = new StringCalculator();
      String[] result = stringCalculator.split("10,20"); 
      System.out.println(result);//["10", "20"]
    }
}
```
```java
class Application {
  static void main() {
    StringCalculator stringCalculator = new StringCalculator();
    String[] result = stringCalculator.split("//;\n30;40");
    System.out.println(result); //["30", "40"]
  }
}
```
##### int sumOf(String[] input)
* 문자열 타입의 숫자들을 받아 합산
* 숫자가 아닌 문자열이 있을 경우 예외 발생
```java
class Application {
  static void main() {
    StringCalculator stringCalculator = new StringCalculator();
    int result = stringCalculator.sumOf(List);
    System.out.println(result); //30
  }
}
```
##### int calculate(String string)
* 기본적으로 `,`와 `:`를 기준으로 분할해 나오는 숫자들을 합산해 반환
* `//{custom delimiter}\n{string}` 형식으로 입력시 기본 구분자를 대신해 `custom delimiter`를 기준으로 분할. 분할 이후에 나오는 숫자들을 합산해 반환
```java
class Application {
  static void main() {
    StringCalculator stringCalculator = new StringCalculator();
    int result = stringCalculator.calculate("10,20");
    System.out.println(result); //30
  }
}
```
```java
class Application {
  static void main() {
    StringCalculator stringCalculator = new StringCalculator();
    int result = stringCalculator.calculate("//;\n30;40");
    System.out.println(result); //70
  }
}
```
```java
class Application {
  static void main() {
    StringCalculator stringCalculator = new StringCalculator();
    int result = stringCalculator.calculate("//;\n30,40"); //Not work (Exception)
    System.out.println(result);
  }
}
```

## Racing

### 기능 요구사항
* 주어진 횟수 동안 n대의 자동차는 전진 또는 멈출 수 있다.
* 각 자동차에 이름을 부여할 수 있다. 전진하는 자동차를 출력할 때 자동차 이름을 같이 출력한다.
* 자동차 이름은 쉼표(,)를 기준으로 구분하며 이름은 5자 이하만 가능하다.
* 사용자는 몇 번의 이동을 할 것인지를 입력할 수 있어야 한다.
* 전진하는 조건은 0에서 9 사이에서 random 값을 구한 후 random 값이 4 이상일 경우 전진하고, 3 이하의 값이면 멈춘다.
* 자동차 경주 게임을 완료한 후 누가 우승했는지를 알려준다. 우승자는 한 명 이상일 수 있다.

### 사용법

#### Car

##### Car(String name) 
* 이름을 받아서 해당 이름을 가진 자동차를 생성하는 생성자
* 이름의 경우 빈칸 혹은 다섯자가 초과할 경우 예외 발생
```java
class Application {
  static void main() {
    Car car = new Car("1");
  }
}
```
```java
class Application {
  static void main() {
    Car car = new Car(""); //Not work (Exception)
  }
}
```
```java
class Application {
  static void main() {
    Car car = new Car("123456"); //Not work (Exception)
  }
}
```

##### void moveByCondition(int value)
* `value`가 `3` 초과일 경우 자동차가 전진
* `3` 이하일 경우 정지
```java
class Application {
  static void main() {
    Car car = new Car("TEST");
    car.moveByCondition(4); //Move
    car.moveByCondition(3); //Not move
  }
}
```

##### String getName()
* 자동차의 이름을 반환하는 함수
```java
class Application {
  static void main() {
    Car car = new Car("TEST1");
    car.getName(); //"TEST1"
  }
}

```

##### String toString()
* 자동차의 이름과 위치를 문자열로 반환하는 함수
* `{name} : {position}` 방식으로 반환
* `{position}`의 경우 `--`, `----`와 같은 방식으로 출력됨
* 예시
  * `ABCD : ---`
  * `1234 : -----`
```java
class Application {
  static void main() {
    Car car = new Car("TEST2");
    car.moveByCondition(4); //Move
    System.out.println(car); //"TEST2 : --"
  }
}
```
```java
class Application {
  static void main() {
    Car car = new Car("TEST3");
    car.moveByCondition(4); //Move
    car.moveByCondition(5); //Move
    car.moveByCondition(6); //Move
    System.out.println(car); //"TEST3 : ----"
  }
}
```

##### int compareTo(Car other)
* 자동차의 위치를 비교하는 함수
* Comparable<Car> 인터페이스에 의해서 생성
```java
class Application {
  static void main() {
    Car car1 = new Car("1");
    Car car2 = new Car("2");
    car2.moveByCondition(4); //Move
    int result = car1.compareTo(car2);
    System.out.println(result); //-1 
  }
}
  
```
```java
class Application {
  static void main() {
    Car car1 = new Car("1");
    Car car2 = new Car("2");
    car1.moveByCondition(4); //Move
    int result = car1.compareTo(car2);
    System.out.println(result); //1 
  }
}
```
```java
class Application {
  static void main() {
    Car car1 = new Car("1");
    Car car2 = new Car("2");
    int result = car1.compareTo(car2);
    System.out.println(result); //0 
  }
}
```

#### Simulator

##### void create(String names)
* 자동차들을 생성해주는 함수
* 문자열을 받아 `,`를 기준으로 분할해 생성
```java
class Application {
  static void main() {
    Simulator simulator = new Simulator();
    simulator.create("A,B");
  }
}
```

##### void run(Random random)
* 한턴을 진행시키는 함수
* 랜덤 인스턴스를 받아 랜덤한 숫자를 생성해 자동차의 조건으로 사용
```java
class Application {
  static void main() {
    Simulator simulator = new Simulator();
    simulator.create("A,B");
    simulator.run(new Random());
  }
}
```

##### String getWinners()
* 가장 앞서나가는 자동차의 이름 반환
* 승자가 중복일 경우 모두 반환
```java
class Application {
  static void main() {
    Simulator simulator = new Simulator();
    simulator.create("A,B");
    simulator.run(new Random());
    String result = simulator.getWinners();
    System.out.println(result); //"A" or "B" or "A, B"
  }
}
```

##### String toString()
* 현재 시뮬레이터 상황을 문자열로 반환
* `Car`의 `toString()`에 의해 반환되는 문자열을 `\n` 문자로 합쳐서 반환
* 예시
  * ```
    A : ---
    B : --
    C : ----
    ```
```java
class Application {
  static void main() {
    Simulator simulator = new Simulator();
    simulator.create("A,B");
    System.out.println(simulator.toString()); //"A : -\nB : -"
  }
}
```

### 기능목록

#### Car
* 이름 (5자 이하)
* 위치
* 전진
* 위치 비교 (compareTo)
* toString - "name: position"

#### Simulator
* n대의 자동차
* m회 시행
* 시행
* 우승 판별 (여러명 가능)

#### Application
* 입력 / 출력
* 시뮬레이션
