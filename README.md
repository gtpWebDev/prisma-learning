# Prisma learning

Includes step-by-step notes to assist in initialising projects with postgres and prisma in the future

A good start guide for much of this is located [here](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases-node-postgresql)

## Set up the project

Create a folder and navigate into it. Then set up a node project and add the Prima CLI as a development dependency

```bash
npm init y
npm install prisma --save-dev
```

Invoke the Prisma CLI:

```bash
npx prisma
```

Set up the prisma ORM project - adds scheam.prisma and a populated .env file:

```bash
npx prisma init
```

**schema.prisma** is by default setup to connect to the data source through the environment variable. Update the database_url env variable to the following form:

```js
DATABASE_URL =
  "postgresql://<USER>>:<PASSWORD>>@localhost:5432/<MYDB>?schema=public";
```

## Set up a postgres database

This [Odin project link](https://www.theodinproject.com/lessons/nodejs-using-postgresql) has an explanation of how to setup your database locally.

Summarised version to setup a database is:

Run the Postgres shell in the terminal:

```bash
pqsl
```

View current databases (letter L not 1!):

```bash
\l
```

Create a database:
(The semicolons are important in postgres shell!)

```bash
CREATE DATABASE dbname;
```

Change to the new database:

```bash
\c dbname
```

Exit out of postgres shell:

```bash
\q
```

## Create a starting database schema

Something along the lines of...

```js
model Post {
  id        Int      @id @default(autoincrement())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  title     String   @db.VarChar(255)
  content   String?
  published Boolean  @default(false)
  author    User     @relation(fields: [authorId], references: [id])
  authorId  Int
}

model Profile {
  id     Int     @id @default(autoincrement())
  bio    String?
  user   User    @relation(fields: [userId], references: [id])
  userId Int     @unique
}

model User {
  id      Int      @id @default(autoincrement())
  email   String   @unique
  name    String?
  posts   Post[]
  profile Profile?
}
```

Then map the data model to the database schema in the client. This basically does the heavy lifting of updating the database model in postgres to match that of the schema. CHeck out the migration.sql file for an understanding.

```bash
npx prisma migrate dev --name init
```

## Install Prisma CLient
