# Реалізація об'єктно-реляційного відображення

## Мета роботи

Реалізувати Java-додаток, що підключається до бази даних MySQL для управління користувачами в контексті "Системи управління відкритими даними". Завданням було створити таблиці, підключитися до них з Java через JDBC, реалізувати патерн DAO (Data Access Object) та протестувати вставку і зчитування даних.

---

## Етапи реалізації

### 1. Створення бази даних та таблиць

Базу `my_database` було створено в MySQL, з таблицями:

- `Role` — ролі користувачів (адмін, аналітик, громадянин)
- `User` — самі користувачі, з `roleId` як зовнішнім ключем

```sql
CREATE DATABASE IF NOT EXISTS opendata;
USE my_database;

CREATE TABLE Role (
    id CHAR(36) PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE User (
    id CHAR(36) PRIMARY KEY,
    name TEXT NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password TEXT NOT NULL,
    roleId CHAR(36),
    FOREIGN KEY (roleId) REFERENCES Role(id) ON DELETE SET NULL
);
```

#### Початкові дані для таблиці `Role`:

```sql
INSERT INTO Role (id, name) VALUES
  ('r-admin', 'Administrator'),
  ('r-analyst', 'Data Analyst'),
  ('r-citizen', 'Citizen');
```

---

### 2. Створення моделі користувача

```java
package com.example.model;

public class User {
    private String id;
    private String name;
    private String email;
    private String password;
    private String roleId;

    // Конструктор, геттери та сеттери...
}
```

**Призначення:** ця модель відображає кожен запис таблиці `User` у вигляді Java-об’єкта.

---

### 3. Опис інтерфейсу DAO

```java
package com.example.dao;

public interface UserDAO {
    User getUserById(String id);
    List<User> getAllUsers();
    void insertUser(User user);
    void updateUser(User user);
    void deleteUser(String id);
}
```

**Призначення:** описує абстрактний набір методів для доступу до таблиці `User`. Завдяки цьому реалізацію можна змінювати незалежно від решти коду.

---

### 4. Реалізація DAO через JDBC

```java
package com.example.dao;

public class UserDAOImpl implements UserDAO {
    @Override
    public void insertUser(User user) {
        String query = "INSERT INTO User (id, name, email, password, roleId) VALUES (?, ?, ?, ?, ?)";
        ...
    }

    @Override
    public List<User> getAllUsers() {
        String query = "SELECT * FROM User";
        ...
    }
}
```

**Призначення:** повноцінна реалізація всіх методів, які напряму звертаються до бази через SQL-запити.

---

### 5. Підключення до бази через утиліту

```java
package com.example.util;

public class DatabaseConnection {
    private static final String URL = "jdbc:mysql://localhost:3306/opendata";
    private static final String USER = "root";
    private static final String PASSWORD = "password";

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(URL, USER, PASSWORD);
    }
}
```

**Призначення:** централізоване управління з’єднанням з MySQL.

---

### 6. Клас Main для тестування

```java
package com.example.dao;

public class Main {
    public static void main(String[] args) {
        UserDAO userDAO = new UserDAOImpl();

        User newUser = new User("u-test1", "Тестовий Користувач", "test@example.com", "test123", "r-citizen");
        userDAO.insertUser(newUser);

        List<User> users = userDAO.getAllUsers();
        for (User user : users) {
            System.out.println(user.getName() + " | " + user.getEmail());
        }
    }
}
```

**Призначення:** демонструє вставку одного користувача та вивід усіх.

---

## Скріншоти роботи

[!Diagram](added-user.png)
[!Diagram](result-in-sql.png)

---

## Висновок

У межах цього проєкту було:
- побудовано структуру бази даних
- створено таблиці з зовнішніми ключами
- реалізовано шаблон DAO для класу `User`
- забезпечено підключення до БД через `DatabaseConnection`
- протестовано вставку та зчитування через клас `Main`

---