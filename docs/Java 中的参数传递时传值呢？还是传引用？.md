在 Java 中，参数传递机制是 **按值传递**（Pass by Value），而**不是**按引用传递（Pass by Reference）。这是 Java 参数传递机制的一个重要特性，但这个概念有时会因为对象引用的处理方式而让人产生混淆。我们来详细分析一下。

### 1. **基本数据类型的传值**
对于 **基本数据类型**（如 `int`, `double`, `char`, `boolean` 等），Java 通过**按值传递**的方式传递参数，即传递的是实际值的副本。

**示例：**

```java
public class Main {
    public static void changeValue(int num) {
        num = 100;  // 这里修改的是 num 的副本
    }

    public static void main(String[] args) {
        int a = 10;
        changeValue(a);
        System.out.println(a);  // 输出 10，a 的值未改变
    }
}
```

在这个例子中，`changeValue` 方法中修改的是 `a` 的副本，原始变量 `a` 不会受到影响，因此输出仍为 `10`。

### 2. **引用类型的传值**
对于 **引用类型**（如对象、数组等），Java 仍然是**按值传递**。但是，传递的**值是对象引用的副本**。这意味着方法得到的是**对象的内存地址的副本**，通过这个副本仍然可以修改对象的内部状态，但无法改变对象的引用本身。

**示例：**

```java
class Person {
    String name;

    Person(String name) {
        this.name = name;
    }
}

public class Main {
    public static void changeName(Person person) {
        person.name = "New Name";  // 修改对象的属性
    }

    public static void main(String[] args) {
        Person p = new Person("Original Name");
        changeName(p);
        System.out.println(p.name);  // 输出 "New Name"
    }
}
```

在这个例子中，传递给 `changeName` 方法的参数是 `person` 的引用副本，通过这个副本，修改了 `Person` 对象的 `name` 属性，最终输出 `"New Name"`。但是请注意，我们并没有改变 `person` 的引用，只是修改了对象的内部状态。

### 3. **不能改变对象的引用本身**
虽然你可以通过传递的引用副本修改对象的内容，但你不能修改对象引用本身，让它指向一个新的对象。

**示例：**

```java
public class Main {
    public static void changePerson(Person person) {
        person = new Person("New Person");  // 尝试改变对象引用
    }

    public static void main(String[] args) {
        Person p = new Person("Original Person");
        changePerson(p);
        System.out.println(p.name);  // 输出 "Original Person"
    }
}
```

在这个例子中，`changePerson` 方法试图将 `person` 引用指向一个新的 `Person` 对象，但这种修改仅仅影响了 `person` 的副本，原始的 `p` 引用并不会改变。因此，最终输出的仍然是 `"Original Person"`。

### 结论
- **基本数据类型**：按值传递，即传递的是值的副本。
- **引用类型**：按值传递，传递的是对象引用的副本（即对象的内存地址）。可以通过引用修改对象的内部状态，但不能改变对象的引用本身。

简单来说，**Java 只有按值传递**，不论是基本类型还是引用类型。