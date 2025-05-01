# Проєктування бази даних

В рамках проекту розробляється: 
- модель бізнес-об'єктів 
- ER-модель
- реляційна схема

# Модель бізнес-об'єктів

@startuml entity User <> #88FFAE entity User.id <> #FFFFFF entity User.login <> #FFFFFF entity User.firstname <> #FFFFFF entity User.lastname <> #FFFFFF entity User.email <> #FFFFFF entity User.password <> #FFFFFF

entity Role <<ENTITY>> #88FFAE
entity Role.id <<NUMBER>> #FFFFFF
entity Role.name <<TEXT>> #FFFFFF

entity Permission <<ENTITY>> #88FFAE
entity Permission.id <<NUMBER>> #FFFFFF
entity Permission.name <<TEXT>> #FFFFFF

entity Grant_Permission <<ENTITY>> #88FFAE

entity Request <<ENTITY>> #88FFAE
entity Request.id <<NUMBER>> #FFFFFF
entity Request.type <<NUMBER>> #FFFFFF
entity Request.message <<TEXT>> #FFFFFF

entity Access <<ENTITY>> #88FFAE
entity Access.id <<NUMBER>> #FFFFFF

entity Datafile <<ENTITY>> #88FFAE
entity Datafile.id <<NUMBER>> #FFFFFF
entity Datafile.name <<TEXT>> #FFFFFF
entity Datafile.content <<TEXT>> #FFFFFF
entity Datafile.description <<TEXT>> #FFFFFF
entity Datafile.format <<TEXT>> #FFFFFF
entity Datafile.date <<DATETIME>> #FFFFFF

entity Tag <<ENTITY>> #88FFAE
entity Tag.id <<NUMBER>> #FFFFFF
entity Tag.name <<TEXT>> #FFFFFF

entity Datafile_tag <<ENTITY>> #88FFAE

User *-u- User.id
User *-u- User.login
User *-u- User.firstname
User *-u- User.lastname
User *-u- User.email
User *-u- User.password

Role *-d- Role.id
Role *-d- Role.name

Permission *-u- Permission.id
Permission *-u- Permission.name

Request *-u- Request.id
Request *-u- Request.type
Request *-u- Request.message

Access *-d- Access.id

Datafile *-d-- Datafile.id
Datafile *-d-- Datafile.name
Datafile *-d-- Datafile.content
Datafile *-d-- Datafile.date
Datafile *-d-- Datafile.description
Datafile *-d-- Datafile.format

Tag *-u- Tag.id
Tag *-r- Tag.name

User "0,*"-d-"1,1" Role

Permission "1,1"-r-"0,*" Grant_Permission
Grant_Permission "0,*"-r-"1,1" Role

User "1,1"-d-"0,*" Access

User "1,1"-u-"0,*" Request

Request "0,*"-r-"1,1" Access

Access "0,*"-r-"1,1" Datafile

Datafile "1,1"-r-"0,*" Datafile_tag
Datafile_tag "0,*"-r-"1,1" Tag

@enduml



# ER-модель

@startuml

entity User  {
+ id: UUID
+ name: TEXT
+ email: TEXT
+ password: TEXT
+ roleId: UUID
}

entity Role  {
+ id: UUID
+ name: TEXT
}

entity Datarecord  {
+ id: UUID
+ name: TEXT
+ data: TEXT
+ type: TEXT
+ time: DATETIME
+ description: TEXT
}

entity Access  {
+ id: UUID
+ userId: UUID
+ datarecordId: UUID
+ time: DATETIME
+ type: TEXT
}

entity Tag  {
+ id: UUID
+ name: TEXT
}

entity Category  {
+ id: UUID
+ name: TEXT
+ parentCategoryId: UUID
}

entity DatarecordTag  {
+ id: UUID
+ datarecordId: UUID
+ tagId: UUID
}

entity DatarecordCategory  {
+ id: UUID
+ datarecordId: UUID
+ categoryId: UUID
}

Role "1.1"-r->"0.*" User
User "1.1"-->"0.*" Access
Access "0.*"-->"1.1" Datarecord
Tag "1.1"-d->"0.*" DatarecordTag
DatarecordTag "0.*"-l->"1.1" Datarecord
Category "1.1"-d->"0.*" DatarecordCategory
DatarecordCategory "0.*"-r->"1.1" Datarecord
Category "0.1"-->"0.*" Category

@enduml

# Реляційна схема 
![Diagram](https://github.com/user-attachments/assets/c164ab09-60de-48c0-b914-b12b7f82fcc0)
