# JDBC-Lab

### Step 1: Set Up Your Environment

Make sure you have the following in place:

1. JDK installed
2. A Java IDE (like IntelliJ or Eclipse)
3. PostgreSQL installed
4. PostgreSQL JDBC driver (JAR file) added to your project

### Step 2: Create a Database and Table

Assuming you have PostgreSQL installed, create a database and a table for this tutorial. You can use the following SQL commands:

```sql
CREATE DATABASE tutorialdb;
\c tutorialdb;

CREATE TABLE person (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    age INT
);
```

### Step 3: Java Project Setup

Create a new Java project in your IDE and add the PostgreSQL JDBC driver JAR file to your project.

### Step 4: Write JDBC Code

#### 4.1. Connection Setup

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DatabaseConnector {
    private static final String URL = "jdbc:postgresql://localhost:5432/tutorialdb";
    private static final String USER = "your_username";
    private static final String PASSWORD = "your_password";

    public static Connection connect() throws SQLException {
        return DriverManager.getConnection(URL, USER, PASSWORD);
    }
}
```

#### 4.2. Create Operation

```java
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class CreateOperation {
    public static void main(String[] args) {
        try (Connection connection = DatabaseConnector.connect()) {
            String sql = "INSERT INTO person (name, age) VALUES (?, ?)";
            try (PreparedStatement preparedStatement = connection.prepareStatement(sql)) {
                preparedStatement.setString(1, "John Doe");
                preparedStatement.setInt(2, 25);
                int rowsAffected = preparedStatement.executeUpdate();
                System.out.println(rowsAffected + " row(s) inserted successfully!");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

#### 4.3. Read Operation

```java
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class ReadOperation {
    public static void main(String[] args) {
        try (Connection connection = DatabaseConnector.connect()) {
            String sql = "SELECT * FROM person";
            try (PreparedStatement preparedStatement = connection.prepareStatement(sql)) {
                ResultSet resultSet = preparedStatement.executeQuery();
                while (resultSet.next()) {
                    System.out.println("ID: " + resultSet.getInt("id") +
                            ", Name: " + resultSet.getString("name") +
                            ", Age: " + resultSet.getInt("age"));
                }
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

#### 4.4. Update Operation

```java
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class UpdateOperation {
    public static void main(String[] args) {
        try (Connection connection = DatabaseConnector.connect()) {
            String sql = "UPDATE person SET age = ? WHERE name = ?";
            try (PreparedStatement preparedStatement = connection.prepareStatement(sql)) {
                preparedStatement.setInt(1, 30);
                preparedStatement.setString(2, "John Doe");
                int rowsAffected = preparedStatement.executeUpdate();
                System.out.println(rowsAffected + " row(s) updated successfully!");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

#### 4.5. Delete Operation

```java
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class DeleteOperation {
    public static void main(String[] args) {
        try (Connection connection = DatabaseConnector.connect()) {
            String sql = "DELETE FROM person WHERE name = ?";
            try (PreparedStatement preparedStatement = connection.prepareStatement(sql)) {
                preparedStatement.setString(1, "John Doe");
                int rowsAffected = preparedStatement.executeUpdate();
                System.out.println(rowsAffected + " row(s) deleted successfully!");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

### Step 5: Combine Operations into Use Cases

Now, you can create use cases by combining the above operations as needed for your application.

```java
public class CombinedUseCases {
    public static void main(String[] args) {
        // Example: Create a person
        CreateOperation.main(args);

        // Example: Read all persons
        ReadOperation.main(args);

        // Example: Update person's age
        UpdateOperation.main(args);

        // Example: Read all persons after update
        ReadOperation.main(args);

        // Example: Delete a person
        DeleteOperation.main(args);

        // Example: Read all persons after delete
        ReadOperation.main(args);
    }
}
```

# Usecase

**Scenario: Address Book Application**

You are developing an address book application that allows users to manage their contacts. The application uses a PostgreSQL database to store contact information. The database has a table named "person" with columns: `id`, `name`, `age`, and `email`. You have implemented basic CRUD operations using JDBC as described in the tutorial.

#### Use Case:

1. **Create Contact:**
   - Implement a Java program to add a new contact named "Bob Smith" with age 35 and email "bob@example.com" to the address book.

2. **Read Contacts:**
   - Develop a Java program to retrieve and display all contacts from the address book.

3. **Update Contact:**
   - Modify an existing Java program to update the age of the contact with the name "Bob Smith" to 40.

4. **Display Updated Contacts:**
   - Write a Java program to fetch and display all contacts after the update.

5. **Delete Contact:**
   - Create a Java program to delete the contact with the email "bob@example.com" from the address book.

6. **Display Remaining Contacts:**
   - Develop a Java program to fetch and display all contacts after the deletion.

#### Instructions:

- Use the provided JDBC code and modify it as needed for each use case.
- Ensure that each program runs independently and displays appropriate success messages or information.
- Test your programs by running them and verifying the changes in the database.
- Pay attention to exception handling to handle potential errors gracefully.
