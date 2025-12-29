# MVC设计模式教学指导

## 🎯 项目概述

本项目是一个基于MVC设计模式的图书管理系统教学版本，专门设计用于帮助学生理解和实践MVC架构、DAO模式和分层开发思想。

## 📁 项目结构

```
src/com/library/
├── Main.java                    # 主程序入口（包含详细教学说明）
├── model/                       # 数据模型层
│   ├── Category.java           # ✅ 完整示例
│   ├── Book.java               # 📝 框架代码
│   ├── User.java               # 📝 框架代码
│   ├── UserRole.java           # ✅ 完整示例
│   └── BorrowRecord.java       # 📝 框架代码
├── dao/                         # 数据访问层
│   ├── DAOFactory.java         # ✅ 工厂类（含状态管理）
│   ├── ICategoryDAO.java      # ✅ 接口定义
│   ├── CategoryDAO.java       # ✅ 完整实现
│   ├── IBookDAO.java         # 📝 接口定义
│   ├── BookDAO.java          # 📝 框架代码
│   ├── IUserDAO.java         # 📝 接口定义
│   ├── UserDAO.java          # 📝 框架代码
│   ├── IBorrowDAO.java       # 📝 接口定义
│   └── BorrowDAO.java        # 📝 框架代码
├── service/                     # 业务逻辑层
│   ├── CategoryService.java   # ✅ 完整实现
│   ├── BookService.java       # 📝 框架代码
│   ├── AuthService.java       # 📝 框架代码
│   └── BorrowService.java    # 📝 框架代码
├── controller/                  # 控制层
│   ├── CategoryController.java # ✅ 完整实现
│   ├── BookController.java    # 📝 框架代码
│   ├── UserController.java    # ❌ 待创建
│   └── BorrowController.java # ❌ 待创建
├── view/                        # 视图层（可选）
│   └── （学生根据需要创建）
└── util/                        # 工具类
    ├── DatabaseHelper.java     # ✅ 完整实现
    ├── InputUtil.java         # ✅ 完整实现
    └── DateUtil.java          # ✅ 完整实现
```

## 🎓 学习路径

### 阶段一：理解MVC架构（推荐学习时间：2-3小时）

1. **运行系统**
   ```bash
   java com.library.Main
   ```

2. **学习分类模块**
   - 运行分类管理功能
   - 添加、修改、删除分类
   - 理解各层之间的调用关系

3. **分析代码结构**
   - Model层：Category.java（数据模型）
   - DAO层：ICategoryDAO.java + CategoryDAO.java（数据访问）
   - Service层：CategoryService.java（业务逻辑）
   - Controller层：CategoryController.java（用户交互）

### 阶段二：实现图书管理模块（推荐学习时间：4-6小时）

1. **Model层实现**
   ```java
   // 完成 Book.java 的所有TODO部分
   - 属性定义
   - 构造函数
   - Getter/Setter方法
   - 业务方法（isAvailable(), isValid()等）
   - 重写方法（toString(), equals(), hashCode()）
   ```

2. **DAO层实现**
   ```java
   // 1. 完成 BookDAO.java 的所有TODO部分
   // 2. 实现IBookDAO接口定义的所有方法
   // 3. 重点学习SQL语句设计和PreparedStatement使用
   // 4. 正确的异常处理和资源管理
   ```

3. **更新DAOFactory**
   ```java
   // 在DAOFactory.java中取消BookDAO相关代码的注释
   bookDAO = new BookDAO();
   ```

4. **Service层实现**
   ```java
   // 完成 BookService.java 的所有TODO部分
   - 业务规则验证
   - 调用DAO层
   - 异常处理
   ```

5. **Controller层实现**
   ```java
   // 完成 BookController.java 的所有TODO部分
   - 用户界面展示
   - 输入处理
   - 调用Service层
   - 结果显示
   ```

6. **集成到主程序**
   ```java
   // 在Main.java中添加图书管理入口
   ```

### 阶段三：实现用户管理模块（推荐学习时间：3-5小时）

参照图书模块的实现步骤，完成：
- User.java
- IUserDAO.java
- UserDAO.java
- UserService.java（可复用AuthService.java）
- UserController.java

### 阶段四：实现借阅管理模块（推荐学习时间：5-7小时）

这是最复杂的模块，需要掌握：
- 借阅业务规则
- 事务处理概念
- 复杂的SQL查询
- 状态管理

## 🔧 关键技术点

### 1. MVC设计模式

```
用户请求 → Controller → Service → DAO → Database
    ↑           ↓          ↓       ↓
   View ← Service ← DAO ← Database
```

- **Controller**：处理用户输入，调用Service
- **Service**：业务逻辑处理，调用DAO
- **DAO**：数据访问，操作数据库
- **Model**：数据模型，在各层间传递

### 2. DAO模式

```java
// 接口定义操作
public interface IBookDAO {
    boolean addBook(Book book);
    List<Book> getAllBooks();
    // ...
}

// 具体实现
public class BookDAO implements IBookDAO {
    // 实现所有接口方法
}
```

### 3. 工厂模式

```java
// 统一创建DAO实例
IBookDAO bookDAO = DAOFactory.getBookDAO();
```

### 4. 数据库操作要点

```java
// 使用PreparedStatement防止SQL注入
String sql = "INSERT INTO books (title, author) VALUES (?, ?)";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, book.getTitle());
pstmt.setString(2, book.getAuthor());

// 正确的资源管理
try {
    // 数据库操作
} finally {
    DatabaseHelper.closeResources(rs, pstmt, conn);
}
```

## 📚 学习建议

### 1. 循序渐进
- 先理解简单的分类模块
- 再实现复杂的借阅模块
- 不要一次性尝试实现所有功能

### 2. 动手实践
- 不要只看代码，一定要动手写
- 遇到问题时先检查分类模块的类似实现
- 多使用调试功能理解程序执行流程

### 3. 理解原理
- 不仅要实现功能，更要理解为什么这样设计
- 思考MVC各层的职责分工
- 理解接口编程的优势

### 4. 代码质量
- 添加必要的注释
- 统一的命名规范
- 正确的异常处理
- 合理的代码组织

## 🎯 评估标准

### 代码完整性（40%）
- 所有TODO部分是否完成
- 功能是否正常运行
- 边界条件是否处理

### 架构设计（30%）
- 是否遵循MVC分层原则
- 各层职责是否清晰
- 接口设计是否合理

### 代码质量（20%）
- 代码可读性
- 异常处理
- 资源管理
- 命名规范

### 文档和注释（10%）
- 代码注释是否充分
- 实现思路是否清晰

## ❓ 常见问题

### Q: 如何开始？
A: 先运行程序，学习分类模块，然后参照实现图书模块。

### Q: 数据库连接失败？
A: 检查db.properties配置，确保MySQL服务已启动。

### Q: 实现完成后如何测试？
A: 运行程序，选择相应功能模块，测试各项操作。

### Q: 可以修改代码结构吗？
A: 可以，但要保持MVC分层原则。

### Q: 需要实现View层吗？
A: 当前版本Controller层包含了简单的用户界面，View层为可选扩展。

## 🔗 参考资源

- [MVC设计模式详解](https://www.runoob.com/design-pattern/mvc-pattern.html)
- [Java编程规范](https://www.oracle.com/technetwork/java/codeconventions-150003.pdf)
- [MySQL官方文档](https://dev.mysql.com/doc/)
- [JDBC教程](https://www.runoob.com/jdbc/jdbc-tutorial.html)

---

**💡 记住**：学习编程最重要的是动手实践，遇到困难是正常的，坚持下去你一定能掌握MVC架构！