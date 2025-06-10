# Prisma Notes

## What is Prisma?
Prisma ORM is a modern, open-source tool that simplifies database management in web development. Acting as a bridge between your code and the database, Prisma provides a type-safe way to query, manipulate, and migrate data. It’s particularly powerful in the JavaScript and TypeScript ecosystems and supports popular databases like PostgreSQL, MySQL, SQLite, and MongoDB.

## Key Features
1. **Type-Safety with TypeScript**
Prisma automatically generates TypeScript types based on your database schema, which reduces bugs and runtime errors by ensuring your queries are validated during compilation.
2. **Easy Database Migrations**
Managing database schema changes becomes simpler with Prisma’s migration system. You can create and apply migrations seamlessly based on changes in your Prisma schema file.
3. **Auto-Generated Client**
Prisma generates an intuitive client that abstracts complex SQL queries, allowing developers to focus on business logic rather than database syntax.
4. **Multi-Database Support**
Prisma supports a wide range of databases, including PostgreSQL, MySQL, SQLite, and MongoDB. This flexibility makes it a versatile tool for developers working across different database environments.
5. **Improved Developer Experience**
Tools like Prisma Studio (a web interface for database management) and built-in relations handling make it easy to manage and visualize your data.

##Keywords
-Data Models
- Field types
- Relations
- Migrations
- Prisma Client
- Prisma Studio
- Type Safety
-Attributes 
- Prisma Schema

## Parts of a  Field
- **Field Name**: The name of the field in the model.
-**Data Type**: The type of data the field holds, such as `String`, `Int`, `Boolean`, etc.
- **Field Type**: The data type of the field (e.g., `String`, `Int`, `Boolean`).
- **Attributes**: Additional properties that modify the field's behavior, such as `@id`, `@unique`, `@default`, etc.

## Field Modifiers
indicate whether a field is required, using **[]** or optional using **?**, unique, or has a default value. Here are some common modifiers:

## Field Attributes
- **@id**: Marks the field as the primary key of the model.
- **@unique**: Ensures that the field's value is unique across all records in the model.
- **@default**: Sets a default value for the field if none is provided.
- **@relation**: Defines relationships between models, specifying how they are connected.
- **@map**: Maps the field to a different name in the database.
- **updatedAt**: Automatically sets the field to the current timestamp whenever the record is updated.
-**@@id**: Used to define a composite primary key for the model, combining multiple fields.
- **@@unique**: Used to define a composite unique constraint across multiple fields in the model.
- **@@map**: Maps the model to a different name in the database.

## Field Types
- **String**: Represents text data.
- **Int**: Represents integer values.
- **Boolean**: Represents true/false values.
- **DateTime**: Represents date and time values.
- **Float**: Represents floating-point numbers.
- **Json**: Represents JSON data, allowing for flexible data structures.
- **Bytes**: Represents binary data.
- **uuid**: Represents universally unique identifiers (UUIDs).
- **Decimal**: Represents decimal numbers with fixed precision, useful for financial calculations.
- **BigInt**: Represents large integer values that exceed the range of standard integers.

## How to Use Prisma ORM
### Installation
First, initialize a Node.js project if you haven’t already:
```
npm init -y
npm install prisma --save-dev
```

Next, initialize Prisma:
```
npx prisma init --datasource-provoider DATABASE
```
Replace Database with the type of database you are using (e.g., `postgresql`, `mysql`, `sqlite`, etc.).

```
npx prisma init --datasource-provider postgresql
```
Prisma uses postgresql by default so you can skip the `--datasource-provider` flag if you are using PostgreSQL.

```
   npx prisma init
```
this command does two things:

1. This will create a prisma directory and a schema.prisma file where you define your database schema.

2. It will also create a `.env` file where you can set your database connection string.

### Defining Your Schema
In the `schema.prisma` file, you define your data models. Here’s an example of a simple User model:

```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id    Int     @id @default(autoincrement())
  email String  @unique
  name  String
  posts Post[]
}

model Post {
  id       Int    @id @default(autoincrement())
  title    String
  content  String
  authorId Int
  author   User   @relation(fields: [authorId], references: [id])
}
```

### Connect to the database
In the .env file, define your database connection string:
```
DATABASE_URL="postgresql://username:password@localhost:5432/mydatabase"
```

### RunMigrations
In Prisma, migrations are a way to manage and apply changes to your database in a controlled and consistent way.

Run the migration to apply the schema to your database:
```
npx prisma migrate dev --name MIgrationName
```
Anytime you make changes to your schema, you can create a new migration with a descriptive name. For example, if you add a new field to the User model, you would run:
```
npx prisma migrate dev --name add-new-field
```
### Generate Prisma client
The Prisma Client is an auto-generated and type-safe database client that you use to interact with your database in a Node.js or TypeScript application. It’s generated based on the models you define in your Prisma schema (schema.prisma) and provides a simple, intuitive, and type-safe API for CRUD operations, filtering, pagination, and more.

To generate the Prisma Client, run the following command:

```
npx prisma generate
```

During migration, Prisma will automatically generate the Prisma Client based on your schema.
If not installed, you can install it using:
```
npm install @prisma/client
```

### Querying the Database

You can now query the database using the generated client:

```javascript
const {PrismaClient} = require('@prisma/client');
const prisma = new PrismaClient();

async function main() {
  const newUser = await prisma.user.create({
    data: {
      email: 'chepkuruiprudence4032gmail.com',
      name: 'Prudence Chepkurui',
      posts: {
        create: { title: 'Prudences world', content: 'Walk with me this journey!'
         },
      },
    },
  });
  console.log('Created new user:', newUser);

  main()
  .catch((e) => {
    console.error(e);
  })
  .finally(async () => {
    await prisma.$disconnect();
  });
}
main();
```

# CRUD Operations 
## Create
```
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();

const createUser = async () => {
  const newUser = await prisma.user.create({
    data: {
      email: 'pruddy@gmail.com',
      name: 'Prudence Chepkurui',
    }
  });
  console.log('createUser);
}
createUser();
```

### Use createMany to create multiple records at once

```
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();
const createMultipleUsers = async () => {
  const users = await prisma.user.createMany({
    data: [
      { email: 'morristom@gmail.com', name: 'Morris Tom' },
      { email: 'chela2mail.com', name: 'Chela Chepkurui' },
      { email: 'oness@gmail.com', name: 'Oness Chepkurui' },
    ],
  });
  console.log('createMultipleUsers');
}
createMultipleusers();
```

## Read
```
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();

const readUsers = async () => {
  const users = await prisma.user.findMany();
  console.log('readUsers:', users);
}
readUsers();
```

## Update
```
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();

const updateUser = async () => {
  const updatedUser = await prisma.user.update({
    where: { email: 'chepkuruio@gmail.com' },
    data: { name: 'Prudence Chepkurui Updated' },
  });
  console.log('updateUser:', updatedUser);
}
updateUser();
```

## Delete
```
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();

const deleteUser = async () => {
  const deletedUser = await prisma.user.delete({
    where: { email: 'chepkutui@gmail.com' },
  });
  console.log('deleteUser:', deletedUser);
}
deleteUser();
```

#Relationships
## One-to-Many Relationship
In Prisma, a one-to-many relationship is defined using relation fields. For example, if a User can have multiple Posts, you can define it like this:

```prisma
model User {
  id    Int     @id @default(autoincrement())
  email String  @unique
  name  String
  posts Post[] 
}
model Post {
  id       Int    @id @default(autoincrement())
  title    String
  content  String
  authorId Int
  author   User   @relation(fields: [authorId], references: [id]) 
}
```

## Many-to-Many Relationship
In Prisma, a many-to-many relationship is defined using a join table. For example, if Users can have multiple Roles and Roles can belong to multiple Users, you can define it like this:

```prisma
model User {
  id    Int     @id @default(autoincrement())
  email String  @unique
  name  String
  roles Role[] @relation("UserRoles")
}
model Role {
  id    Int     @id @default(autoincrement())
  name  String
  users User[] @relation("UserRoles")
}
```
## one to one relationship

In Prisma, a one-to-one relationship is defined using relation fields with a unique constraint. For example, if each User has one Profile and each Profile belongs to one User, you can define it like this:

```prisma
model User {
  id       Int      @id @default(autoincrement())
  email    String   @unique
  name     String
  profile  Profile? @relation(fields: [profileId], references: [id])
  profileId Int?    @unique
}
model Profile {
  id       Int    @id @default(autoincrement())
  bio      String
  user     User   @relation(fields: [userId], references: [id])
  userId   Int    @unique
}
```

 # Benefits of Prisma ORM

1. **Type-Safety:** Prisma eliminates the risk of runtime errors with its type-safe query system.
2. **Effortless Migrations:** Manage database schema changes easily with Prisma Migrate.
3. **Auto-Generated Client:** Skip writing raw SQL by using the Prisma Client, which is automatically tailored to your schema.
4. **Relations Handling:** Prisma simplifies complex relationships between models using relation fields.
5. **Built-In Studio:** Prisma Studio provides a graphical interface to interact with your data visually.

















## Prisma Schema
- The main configuration file for Prisma ORM.
- Consists of:
  - **Data sources**: Define database connection details.
  - **Generators**: Specify what clients/tools to generate (e.g., Prisma Client).
  - **Data models**: Define your application's data structure.

## Getting Started
- Initialize Prisma in your project:
  ```
  npx prisma init --db
  ```
- This sets up the Prisma schema and configuration files.

