一个持久层框架，用来简化数据库操作，集成在Spring中
# 组件解析
1. 配置文件，`mybatis-config.xml`，用于定义数据源、事务管理器以及其他设置
2. SQL映射文件，存放SQL的XML文件，用来告诉MyBatis如何映射到bean或者Java对象
3. `SqlSessionFactory` 是MyBatis初始化时候的工厂类，用于生成`SqlSession`
4. `SqlSession` 是MyBatis实际和数据库交互的对象，这个会话中包含了所有的SQL语句封装，`insert/update/select/delete` 等
5. 映射器，通常使用接口来代表SQL映射，这些接口方法对应了SQL映射文件中的SQL语句，使得应用可以像调用普通方法那样执行SQL语句