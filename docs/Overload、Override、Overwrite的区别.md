在 Java 中，**Overload**、**Override** 和 **Overwrite** 是三个不同的概念，它们分别有着独特的含义和使用场景。让我们逐一分析它们的区别。

### 1. **Overload（重载）**

**方法重载**（Method Overloading）指的是在同一个类中，可以定义多个方法，方法名相同，但**参数列表不同**（参数的类型、数量、或顺序不同）。重载的目的是为了增加代码的灵活性，使方法可以处理不同的输入情况。

- **关键点**：
  - 方法名相同。
  - 参数列表不同（参数类型、数量或顺序不同）。
  - 可以改变返回类型（但不能仅靠返回类型区分重载的方法）。
  - 在同一个类中，或在子类中也是允许的。

**示例**：

```java
class Calculator {
    // 重载的 add 方法
    public int add(int a, int b) {
        return a + b;
    }

    public double add(double a, double b) {
        return a + b;
    }

    public int add(int a, int b, int c) {
        return a + b + c;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println(calc.add(2, 3));         // 输出 5
        System.out.println(calc.add(2.5, 3.5));     // 输出 6.0
        System.out.println(calc.add(2, 3, 4));      // 输出 9
    }
}
```

### 2. **Override（重写）**

**方法重写**（Method Overriding）指的是在**子类**中，提供了与父类中方法**相同的方法签名**（方法名、参数列表、返回类型等完全相同）的实现。重写的目的是为了改变或扩展父类的方法行为，使得子类能够提供自己的实现。

- **关键点**：
  - 必须在子类中重写父类的方法。
  - 方法名、参数列表、返回类型必须与父类的方法完全相同。
  - 重写的方法不能有比父类方法更严格的访问权限（如父类的方法是 `public`，子类不能将其改为 `private`）。
  - 可以使用 `@Override` 注解来明确表明是重写。

**示例**：

```java
class Animal {
    public void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Dog barks");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Animal();
        animal.sound();  // 输出 "Animal makes a sound"

        Animal dog = new Dog();
        dog.sound();     // 输出 "Dog barks" （重写了父类的 sound 方法）
    }
}
```

### 3. **Overwrite（覆盖）**

**Overwrite** 在编程中并不是一个严格的 Java 技术术语。它的字面含义是**覆盖**，通常用于表示覆盖已有的数据、文件或内容。在 Java 代码中，"overwrite" 可能用于描述操作文件、变量赋值时覆盖原有内容的行为，但它不涉及面向对象编程中的方法定义与行为。

例如，在文件操作时，"overwrite" 表示将新内容写入文件，覆盖掉文件的旧内容。

**示例**：

```java
import java.io.FileWriter;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try {
            FileWriter writer = new FileWriter("example.txt");  // 如果文件已存在，将覆盖其内容
            writer.write("This will overwrite the file content");
            writer.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

在这个例子中，文件 `"example.txt"` 的内容被覆盖掉，写入新的内容。

### 区别总结：

| 比较项        | **Overload（重载）**                             | **Override（重写）**                             | **Overwrite（覆盖）**                                 |
|---------------|--------------------------------------------------|-------------------------------------------------|-----------------------------------------------------|
| **定义**      | 在同一个类中，方法名相同，参数列表不同           | 子类中重写父类的方法，方法签名必须相同           | 通常指覆盖数据、文件等内容，不涉及面向对象编程中的方法 |
| **使用场景**  | 增加方法的灵活性，允许方法处理不同的输入         | 改变或扩展父类方法的行为，使子类提供自己的实现   | 覆盖变量、文件等已有的数据                            |
| **参数列表**  | 必须不同（参数类型、数量、顺序）                 | 必须相同                                         | 与方法无关                                           |
| **返回类型**  | 可以不同，但不能仅通过返回类型区分               | 必须相同（或是子类返回类型，Java 5+ 支持协变返回类型） | 无关                                                 |
| **访问权限**  | 没有限制                                         | 子类方法的访问权限不能比父类方法更严格           | 无关                                                 |
| **适用对象**  | 同一类内的多个方法                               | 子类与父类之间的方法                             | 通常用于文件或变量                                   |

### 总结：

- **Overload（重载）**：是在同一个类中，允许使用相同的方法名来处理不同的参数组合。
- **Override（重写）**：是在子类中提供与父类方法相同签名的方法，实现不同的功能。
- **Overwrite（覆盖）**：通常与文件或数据操作相关，用于表示覆盖已有的内容，与面向对象编程无关。

在 Java 中，最常见的概念是**方法重载（Overload）**和**方法重写（Override）**，分别用于提供多种方法实现和修改父类方法的行为。