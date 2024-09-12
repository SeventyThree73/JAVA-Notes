JDK（Java Development Kit）、JRE（Java Runtime Environment）和 JVM（Java Virtual Machine）是 Java 生态系统中密切相关的三部分，但它们各自有不同的角色：

### 1. **JDK（Java Development Kit，Java 开发工具包）**：
   - **作用**：JDK 是用于开发 Java 应用程序的工具包，包含了编写、编译、运行 Java 程序所需的工具。
   - **包含内容**：JDK 包含 JRE（Java 运行环境）、编译器（`javac`），以及其他一些用于开发和调试的工具（如 `javadoc`、`jdb` 等）。
   - **使用场景**：开发者在编写和编译 Java 代码时需要使用 JDK。

### 2. **JRE（Java Runtime Environment，Java 运行环境）**：
   - **作用**：JRE 是用于运行 Java 应用程序的环境，它提供了 Java 程序运行所需的库、类和资源。
   - **包含内容**：JRE 包含 JVM（Java 虚拟机）和标准 Java 类库，但不包括开发工具（如编译器）。
   - **使用场景**：当你只是想运行 Java 程序而不是开发时，JRE 足够。大多数普通用户只需要 JRE 来运行 Java 应用。

### 3. **JVM（Java Virtual Machine，Java 虚拟机）**：
   - **作用**：JVM 是 Java 的核心部分，它负责执行字节码（即编译后的 `.class` 文件）。JVM 提供跨平台能力，使得 Java 程序能够在不同操作系统上运行。
   - **包含内容**：JVM 定义了 Java 程序的运行方式，包括内存管理、垃圾回收、线程管理等。
   - **使用场景**：JVM 是任何 Java 应用程序的运行时环境的核心部分，不论是通过 JDK 还是 JRE 运行 Java 程序，JVM 都是实际执行代码的核心组件。

### 三者关系总结：
- **JVM** 是运行 Java 字节码的核心。
- **JRE** 包含 JVM 和标准类库，用于执行 Java 应用程序。
- **JDK** 包含 JRE 及开发工具，用于编写、编译和调试 Java 程序。

用图来解释的话：
```
JDK = JRE + 开发工具
JRE = JVM + 类库
```

JDK → JRE → JVM，开发使用 JDK，运行时只需 JRE，而核心执行由 JVM 负责。