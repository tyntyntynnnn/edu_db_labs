## 2. SQL-скрипт для створення БД і таблиці
```sql
-- Створення бази даних
CREATE DATABASE IF NOT EXISTS category_db;
USE category_db;
-- Створення таблиці Category
CREATE TABLE Category (
    id CHAR(36) PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    parentCategoryId CHAR(36),
    FOREIGN KEY (parentCategoryId) REFERENCES Category(id) ON DELETE SET NULL
);