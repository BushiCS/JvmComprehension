Мы создаем класс:
* Во время работы IDEA классы.java попадают в компилятор javac, который компилирует в байт-код и создает файлы с расширением .class, которые попадают в JVM
* Далее для корректной загрузки классов используются класслоадеры (их работа: подгрузка -> связывание -> инициализация)
* после этого класс загружается в metaspace, имеющий данные о классе (имена, методы, поля)
***
Далее создаем статичный метод main(), который попадает
в stack и имеющий внутри себя сигнатуры, лежащие в stack:
* `int i = 1;` где int = примитивный тип, i - название переменной, храняющее в себя значение (1), и все это находится в stack;
* `Object o = new Object();`, где `Object` - ссылочный тип объекта (находящийся в стэке) и `o` - название объекта, храняющее в себе ссылку (находящуюся в стэке) на объект `new Object()`, который, в свою очередь, находится в куче (heap)
* `Integer ii = 2;`, где Integer - ссылочный тип объекта (запись аналогична `Integer ii = new Integer(2);`), класс-обертка, работающий аналогично `Object o`
*  вызов метода `printAll(o, i, ii);`, где вызов метода находится в стэке, но объект Integer внутри метода хрантися в куче

Когда вызывается метод `printAll(o, i, ii);`, то он находится в стеке и после отработки убирается из него. Затем отрабатывается метод println аналогичным образом. 
При каждом вызове метода в стеке создается отдельный фрейм, внутри которого сигнатуры метода. 
***
При выполнении программы используется интерпретатор, интерпретирующий байт-код в машинный, а также JIT-компилятор для оптимизации эффективности и GC, выгружающий неиспользуемые более объекты

    public class JvmComprehension {

    public static void main(String[] args) { main попадает в stack (фрейм 1) (инициализация JvmComprehension)
        int i = 1;                      // 1 внутри фрейма 1 переменная i и ее значения попадают в stack
        Object o = new Object();        // 2 класс Object грузится класслоадером, o (внутри ссылка) попадает в stack, new Object() в кучу
        Integer ii = 2;                 // 3 класс Integer грузится класслоадером, ii попадает в stack, объект new Integer(2) в кучу
        printAll(o, i, ii);             // 4 printAll попадает в stack (фрейм 2), сигнатуры в stack (ссылочные со ссылками на объекты, которые в куче) (внутри фрейма 2)
        System.out.println("finished"); // 7 println попадает в stack (фрейм 4), ссылка на строку внутри в stack, данные строки в куче
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5 uselessVar попадает в stack, new Integer(700) в кучу 
        System.out.println(o.toString() + i + ii);  // 6 println попадает в stack (фрейм 3), сигнатуры в stack (ссылочные со ссылками на объекты, которые в куче (внутри фрейма 3)
    }
    }