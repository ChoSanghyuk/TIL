

import java.sql.PreparedStatement





### Class.forName()

When you use `Class.forName()` to load a JDBC driver, you are making the driver's implementation class available to the JVM, so that it can be used to establish a database connection using the `DriverManager` class.

When you load a driver using `Class.forName()`, it registers the driver with the `DriverManager`, so that it becomes available for use in establishing connections to a particular type of database.

JDBC drivers to be automatically loaded and registered with the `DriverManager` based on the contents of the JDBC URL.







### Connection Poll

The `DriverManager` class in Java does not provide built-in support for connection pooling. The `DriverManager` class is a basic utility class that provides a way to register and manage JDBC drivers and establish database connections, but it does not offer advanced features like connection pooling.

To implement connection pooling in a Java application, you would typically need to use a third-party library or a connection pooling framework that provides the necessary features. 



Sure! Here are some official documentation links that provide more information about JDBC and the service provider mechanism:

1. Java SE Documentation - JDBC Overview: https://docs.oracle.com/en/java/javase/14/docs/api/java.sql/java/sql/package-summary.html
2. Java SE Documentation - Loading a JDBC Driver: https://docs.oracle.com/en/java/javase/14/docs/api/java/sql/DriverManager.html
3. Java SE Documentation - JDBC Service Provider Mechanism: https://docs.oracle.com/en/java/javase/14/docs/api/java/sql/Driver.html#registerWithDriverManager()
4. Java SE Tutorial - JDBC Basics: https://docs.oracle.com/en/java/javase/14/docs/api/java/sql/package-summary.html
5. Java SE Tutorial - Connecting to a Database Using JDBC: https://docs.oracle.com/en/java/javase/14/docs/api/java/sql/DriverManager.html
6. JDBC 4.3 Specification - Section 9.2 "JDBC Drivers" and Section 9.2.1 "DriverManager" (available as part of the JDBC API documentation): https://download.oracle.com/otndocs/jcp/jdbc-4_3-mrel3-spec/index.html



### connection pool



If we analyze the sequence of steps involved in a typical database connection life cycle, we'll understand why:

1. Opening a connection to the database using the database driver
2. Opening a [TCP socket](https://en.wikipedia.org/wiki/Network_socket) for reading/writing data
3. Reading / writing data over the socket
4. Closing the connection
5. Closing the socket

It becomes evident that **database connections are fairly expensive operations**, and as such, should be reduced to a minimum in every possible use case (in edge cases, just avoided).

By just simply implementing a database connection container, which allows us to reuse a number of existing connections, we can effectively save the cost of performing a huge number of expensive database trips. 

This boosts the overall performance of our database-driven applications.



##### **3.2. HikariCP**

Now let's look at [HikariCP](https://github.com/brettwooldridge/HikariCP), a lightning-fast JDBC connection pooling framework created by [Brett Wooldridge](https://github.com/brettwooldridge) (for the full details on how to configure and get the most out of HikariCP, please check out[ this article](https://www.baeldung.com/hikaricp)):

```java
public class HikariCPDataSource {
    
    private static HikariConfig config = new HikariConfig();
    private static HikariDataSource ds;
    
    static {
        config.setJdbcUrl("jdbc:h2:mem:test");
        config.setUsername("user");
        config.setPassword("password");
        config.addDataSourceProperty("cachePrepStmts", "true");
        config.addDataSourceProperty("prepStmtCacheSize", "250");
        config.addDataSourceProperty("prepStmtCacheSqlLimit", "2048");
        ds = new HikariDataSource(config);
    }
    
    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }
    
    private HikariCPDataSource(){}
}Copy
```

Similarly, here's how to get a pooled connection with the *HikariCPDataSource* class:

```java
Connection con = HikariCPDataSource.getConnection();
```

https://www.baeldung.com/java-connection-pooling