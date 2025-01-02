# Table of Content

- [Table of Content](#table-of-content)
- [Overview](#overview)
  - [Structure Breakdown](#structure-breakdown)
    - [Types of Relations](#types-of-relations)
    - [Data Types](#data-types)
  - [Commands](#commands)

# Overview

Prisma is an open-source ORM - Object-Relational Mapping tool

prisma/schema.prisma

```prisma
 db {
  provider = "postgresql"
  // Database Provider Eg. postgresql, mysql, sqlite and Etc
  url= env("DATABASE_URL")
  // .env file
}
```

Eg. Nextjs and Postgres

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url = env("POSTGRES_PRISMA_URL")
  directUrl = env("POSTGRES_URL_NON_POOLING") // uses a direct connection
}
```

[Return](#table-of-content)

## Structure Breakdown

Defining Two Models: 1. User 2. Post

```prisma
model User {
  id        String   @default(cuid()) @id // PK
  name      String
  email     String   @unique // Unique Constraint
  posts     Post[]
  createdAt DateTime @default(now())
}

model Post {
  id        String   @default(cuid()) @id // PK
  title     String
  content   String?
  published Boolean  @default(false)
  authorId  Int
  author    User     @relation(fields: [authorId], references: [id]) // FK
  createdAt DateTime @default(now())
}

// Relation: User 1..* Posts
```

- Models: Represented as Tables in the Database; Each Model contains Fields known as Columns in Tables
- `id`: PK - Primary Key
- `cuid()`: Generate Collisions-Resistant Unique Identifiers
- `@unique`: Ensure Unique Values across Records
- `@relation`: Defines Relationships between Models; Eg. Post FK - authorId > User - id

[Return](#table-of-content)

### Types of Relations

1. 1..1 Relationship

    User 1..1 Profile; FK Profile userId -> User id

```prisma
model Profile {
  id       String  @default(cuid()) @id
  bio      String?
  userId   Int     @unique
  user     User    @relation(fields: [userId], references: [id])
}
```

2. 1..* Relationship

    A) User 1..*Post <br> B) Post*..1 User

```prisma
model User {
  id        String   @default(cuid()) @id
  posts     Post[]   // User 1..* Post
}

model Post {
  id        String   @default(cuid()) @id
  authorId  Int
  author    User     @relation(fields: [authorId], references: [id])
  // author defines the relationship
}

// User is the author of a Post
```

1. *..* Relationship

    Student *..* Course Records; Course *..* Student Records

```prisma
model Student {
  id       String   @default(cuid()) @id
  name     String
  courses  Course[] @relation("studentCourses")
}

model Course {
  id       String   @default(cuid()) @id
  name     String
  students Student[] @relation("studentCourses")
}
```

[Return](#table-of-content)

### Data Types

| Type     | Description            | Eg.                       |
| -------- | ---------------------- | ------------------------- |
| Int      | Integer values         | 1 <br> 42                 |
| Float    | Floating-point numbers | 3.14                      |
| String   | Text                   | "Hello"                   |
| Boolean  | Boolean values         | True, False               |
| DateTime | Date and time          | 2021-01-01T00:00:00Z      |
| Json     | JSON data              | { "key": "value" }        |
| Bytes    | Raw binary data        | File data (binary format) |

[Return](#table-of-content)

## Commands

```bash
npx prisma generate

npx prisma db push

npx prisma db pull

npx prisma studio

npx prisma migrate dev --name <migration-name>

npx prisma migrate deploy

npx prisma migrate reset

npx prisma migrate status
```

[Return](#table-of-content)
