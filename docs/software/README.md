# Реалізація інформаційного та програмного забезпечення

## SQL-скрипт для створення та початкового наповнення бази даних

```sql
USE my_database;

SET FOREIGN_KEY_CHECKS = 0;

DROP TABLE IF EXISTS Access;
DROP TABLE IF EXISTS DatarecordCategory;
DROP TABLE IF EXISTS DatarecordTag;
DROP TABLE IF EXISTS User;
DROP TABLE IF EXISTS Role;
DROP TABLE IF EXISTS Datarecord;
DROP TABLE IF EXISTS Category;
DROP TABLE IF EXISTS Tag;

SET FOREIGN_KEY_CHECKS = 1;

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

CREATE TABLE Datarecord (
    id CHAR(36) PRIMARY KEY,
    name TEXT NOT NULL,
    data TEXT NOT NULL,
    type TEXT NOT NULL,
    time TIMESTAMP NOT NULL,
    description TEXT
);

CREATE TABLE  Access (
    id CHAR(36) PRIMARY KEY,
    userId CHAR(36),
    datarecordId CHAR(36),
    time TIMESTAMP NOT NULL,
    type TEXT NOT NULL,
    FOREIGN KEY (userId) REFERENCES User(id) ON DELETE CASCADE,
    FOREIGN KEY (datarecordId) REFERENCES Datarecord(id) ON DELETE CASCADE
);

CREATE TABLE  Tag (
    id CHAR(36) PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE Category (
    id CHAR(36) PRIMARY KEY,
    name TEXT NOT NULL,
    parentCategoryId CHAR(36),
    FOREIGN KEY (parentCategoryId) REFERENCES Category(id) ON DELETE SET NULL
);

CREATE TABLE DatarecordTag (
    id CHAR(36) PRIMARY KEY,
    datarecordId CHAR(36),
    tagId CHAR(36),
    FOREIGN KEY (datarecordId) REFERENCES Datarecord(id) ON DELETE CASCADE,
    FOREIGN KEY (tagId) REFERENCES Tag(id) ON DELETE CASCADE
);

CREATE TABLE  DatarecordCategory (
    id CHAR(36) PRIMARY KEY,
    datarecordId CHAR(36),
    categoryId CHAR(36),
    FOREIGN KEY (datarecordId) REFERENCES Datarecord(id) ON DELETE CASCADE,
    FOREIGN KEY (categoryId) REFERENCES Category(id) ON DELETE CASCADE
);
```

## RESTfull сервіс для управління даними
