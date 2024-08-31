
### 值传递和引用传递

**值传递：** 在值传递中，传递给函数的是实际参数的值 的副本，当在函数内修改这个副本的时候，不会影响到原始值

**引用传递：** 方法接收的是 实际参数所引用的对象在堆中的 地址，不会创建副本，对形式参数的改变将会直接影响到实际参数

**在 Java 中，所有的参数传递都是 值传递，但是 Java 中处理基本类型和对象引用时的表现有所不同。**

对于基本类型（如 `int`、`char`、`boolean` 等），值传递意味着在方法调用时，传递的是该变量的**副本**。因此，在方法内部修改参数的值，不会影响到方法外原始变量的值。例如：

```java
public class Main {
    public static void main(String[] args) {
        int a = 5;
        changeValue(a);
        System.out.println(a); // 输出仍然是 5
    }

    public static void changeValue(int x) {
        x = 10;
    }
}

```

对于对象来说，传递的也是**值**，但这个值是对象引用的**副本**。也就是说，方法内部的参数是对象引用的副本，指向的是原来的对象。因此，在方法内部可以通过这个引用修改对象的状态，但不能修改引用本身（即不能使它指向另一个对象）而影响到方法外的引用。例如：

```java
public class Main {
    public static void main(String[] args) {
        MyObject obj = new MyObject();
        obj.value = 5;
        changeValue(obj);
        System.out.println(obj.value); // 输出 10
    }

    public static void changeValue(MyObject o) {
        o.value = 10; // 修改的是同一个对象的属性
    }
}

class MyObject {
    int value;
}

```

在上面的例子中，`obj` 和 `o` 都指向同一个 `MyObject` 实例，所以修改 `o.value` 会影响到 `obj.value`。

但是，如果尝试改变对象的引用本身，则不会影响到原始的引用：

```java
public class Main {
    public static void main(String[] args) {
        MyObject obj = new MyObject();
        obj.value = 5;
        changeReference(obj);
        System.out.println(obj.value); // 仍然输出 5
    }

    public static void changeReference(MyObject o) {
        o = new MyObject(); // 改变了引用的副本
        o.value = 10;
    }
}

```

在这个例子中，`changeReference` 方法试图改变 `o` 的引用指向另一个新的对象，但 `obj` 仍然指向原来的对象，因而在 `main` 方法中打印的 `value` 仍然是 5。


**总结**： 因此，Java 的参数传递方式都是**值传递**。对于基本类型，传递的是值的副本；对于对象，传递的是对象引用的副本。


## 类和对象


**面向对象：** 强调封装、继承和多态，更容易扩展和维护

**面向过程：** 将系统视为一系列的过程或函数，强调的是算法和流程。

### 面向对象的三大特征

封装

继承

多态


### 编译时多态和运行时多态

编译时多态：方法重载

运行时多态：方法重写

