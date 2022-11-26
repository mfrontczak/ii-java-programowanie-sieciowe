# Lab 2
## Wątki 

Przykład własnej implementacji klasy Thread.
```java
class MyThread extends Thread{
    private final int i;

    MyThread(int i) {
        this.i = i;
    }
    public void run(){
        System.out.println("Wątek " + this.i);
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread th1 = new MyThread(1);
        MyThread th2 = new MyThread(2);

        th1.start();
        th2.start();
        try {
            th1.join();
            th2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```

```java
class Counter {
    private int value;

    public void increment() {
           value += 1;
    }
    public int getValue() {
        return value;
    }
}
```


### Zadania
1. Jaka jest różnica między wątkiem a procesem?
2. Korzystając z metody `sleep` klasy `Thread` uśpij główny wątek na 10s.
3. Utwórz listę wątków (min. 3) korzystając z klasy `Thread`, każdy wątek niech odlicza do 10 i wypisuje numer wątku.
4. Utwórz 2 wątki (liczące do 100_000), zmodyfikuj priorytet jednego z nich. Jak zmieniło się wyjście z programu?
5. Utwórz klasę `Printer`, klasa powinna udostępniać metodę `printNumbers(int mod)` wyświetlającą liczby od `1*mod` do `10*mod` w odstępach czasowych (500ms). Utwórz klasę `MyPrinterThread` przyjmujący jako parametr obiekt klasy `Printer`. Utwórz 3 wątki, każdy powinien uruchamiać metodę `printNumbers` z różnymi wartościami parametru `mod`. 
6. Zmodyfikuj klasę z poprzedniego zadania dodając do metody `printNumbers` słowo kluczowe `synchronized`, co się zmieniło?
7. Zapoznaj się z Interfejsem Funkcyjnym `Runnable`, spróbuj zmodyfikować zadanie 5.
8. Utwórz dwa wątki korzystające z jednego obiektu klasy `Counter`. Jeden wątek powinien wywoływać metodę `increment`, drugi `getValue`. 
9. Zmodyfikuj zadanie 8. Dodaj obiekt klasy `Lock` do wątków w celu synchronizacji między wywołaniami metod.

### W domu
1. https://www.baeldung.com/java-runnable-callable
2. https://www.baeldung.com/java-executor-service-tutorial
