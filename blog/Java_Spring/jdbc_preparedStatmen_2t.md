

```java
import org.apache.commons.dbcp2.BasicDataSource;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class ConnectionPoolExample {
    private static BasicDataSource dataSource; // Connection pool instance

    static {
        // Create and configure the connection pool
        dataSource = new BasicDataSource();
        dataSource.setUrl("jdbc:mysql://localhost/mydb");
        dataSource.setUsername("username");
        dataSource.setPassword("password");
        dataSource.setMinIdle(5);
        dataSource.setMaxIdle(10);
        dataSource.setMaxOpenPreparedStatements(100);
    }

    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;

        try {
            // Obtain a connection from the connection pool
            connection = dataSource.getConnection();

            // Use the connection to create a PreparedStatement
            String sql = "SELECT * FROM users WHERE age > ?";
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setInt(1, 18);

            // Execute the query
            resultSet = preparedStatement.executeQuery();

            // Process the ResultSet
            while (resultSet.next()) {
                // Do something with the results
            }

        } catch (SQLException e) {
            // Handle any exceptions
            e.printStackTrace();
        } finally {
            try {
                // Close the ResultSet, PreparedStatement, and Connection
                if (resultSet != null) resultSet.close();
                if (preparedStatement != null) preparedStatement.close();
                if (connection != null) connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
```

