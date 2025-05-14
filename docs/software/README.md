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

-- ------------------------
-- Створення таблиць
-- ------------------------

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

CREATE TABLE Access (
    id CHAR(36) PRIMARY KEY,
    userId CHAR(36),
    datarecordId CHAR(36),
    time TIMESTAMP NOT NULL,
    type TEXT NOT NULL,
    FOREIGN KEY (userId) REFERENCES User(id) ON DELETE CASCADE,
    FOREIGN KEY (datarecordId) REFERENCES Datarecord(id) ON DELETE CASCADE
);

CREATE TABLE Tag (
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

CREATE TABLE DatarecordCategory (
    id CHAR(36) PRIMARY KEY,
    datarecordId CHAR(36),
    categoryId CHAR(36),
    FOREIGN KEY (datarecordId) REFERENCES Datarecord(id) ON DELETE CASCADE,
    FOREIGN KEY (categoryId) REFERENCES Category(id) ON DELETE CASCADE
);

-- ------------------------
-- Вставка даних (INSERT INTO)
-- ------------------------

-- Ролі
INSERT INTO Role (id, name) VALUES
  ('r-admin', 'Administrator'),
  ('r-analyst', 'Data Analyst'),
  ('r-citizen', 'Citizen');

-- Користувачі
INSERT INTO User (id, name, email, password, roleId) VALUES
  ('u-001', 'Олег Коваль', 'okoval@opendata.gov.ua', 'pass_admin', 'r-admin'),
  ('u-002', 'Марія Іваненко', 'mivanenko@opendata.gov.ua', 'pass_analyst', 'r-analyst'),
  ('u-003', 'Андрій Петренко', 'apetrenko@gmail.com', 'pass_citizen', 'r-citizen');

-- Datarecord
INSERT INTO Datarecord (id, name, data, type, time, description) VALUES
  ('d-001', 'Перелік шкіл', 'JSON data ...', 'json', '2025-05-09 10:00:00', 'Відкритий список усіх шкіл України'),
  ('d-002', 'Дорожні ремонти 2024', 'CSV data ...', 'csv', '2025-04-20 09:30:00', 'Інформація про витрати на ремонти доріг'),
  ('d-003', 'Забруднення повітря', 'XML data ...', 'xml', '2025-03-15 08:00:00', 'Місячний моніторинг якості повітря');

-- Access
INSERT INTO Access (id, userId, datarecordId, time, type) VALUES
  ('a-001', 'u-002', 'd-001', '2025-05-09 11:00:00', 'read'),
  ('a-002', 'u-002', 'd-002', '2025-05-09 11:10:00', 'read'),
  ('a-003', 'u-003', 'd-001', '2025-05-09 12:00:00', 'read');

-- Теги
INSERT INTO Tag (id, name) VALUES
  ('t-edu', 'Освіта'),
  ('t-infra', 'Інфраструктура'),
  ('t-env', 'Екологія');

-- Категорії
INSERT INTO Category (id, name, parentCategoryId) VALUES
  ('c-gov', 'Державні дані', NULL),
  ('c-local', 'Місцеві дані', 'c-gov'),
  ('c-health', 'Охорона здоров’я', 'c-gov');

-- DatarecordTag
INSERT INTO DatarecordTag (id, datarecordId, tagId) VALUES
  ('dt-001', 'd-001', 't-edu'),
  ('dt-002', 'd-002', 't-infra'),
  ('dt-003', 'd-003', 't-env');

-- DatarecordCategory
INSERT INTO DatarecordCategory (id, datarecordId, categoryId) VALUES
  ('dc-001', 'd-001', 'c-local'),
  ('dc-002', 'd-002', 'c-local'),
  ('dc-003', 'd-003', 'c-health');
```

## RESTfull сервіс для управління даними
