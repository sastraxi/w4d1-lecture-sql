# Introduction to Relational Data and SQL

* where do databases fit in the stack?
* serializing to JSON is a type of database
* relational data vs non-relational data
* SQL and declarative programming langauges
  * program the what, not the how

# Example Problem

Let's implement a simple quiz system which contains questions and user's submissions to each question. A quiz can have many multi-choice questions and users can skip answering certain questions.

* **Tables / Entities**: Survey, Questions, Options, Submission, Answers
* Survey has many questions
* Questions have many options (multi choice)
* Questions have many answers
* A submission has a user key (github username or something, no model)
* A submission has many answers

## Schema Design Considerations

* take some time to get it right
* use a tool like [draw.io](https://draw.io) for your ER diagram
* parent-child relationship and where foreign keys go (on the many side!)
* first class foreign key constraints (and `null`ability)
* database normalization
* consistency in naming
  * singular table names in `snake_case`
  * foreign keys named e.g. `table_name_id`
* what if the question text changes? What if the correct answer changes?
* dotted lines between entities that have (indirect) many-many relationships doing to intermediary / join tables
  * same as two one-to-many relationships!

# Queries

* `select` (GET) - grab the values stored in row(s) from some number of tables and return the data
* `insert` (PUT / POST) - creates row(s) in a table
* `update` (PATCH) - updates certain columns for certain row(s) in a table
* `delete` (DELETE) - removes row(s) from a table

# Quick Join Primer

* need access to data in another table? join!
* when joining with a table, you have access to all the columns in all the tables up to that point
* `(type) join <table> on <table>.<col> = <already-selected-column>`
* which new rows correspond to which existing rows?
* e.g. joining from `client`s to `invoice`s
  * client -> invoice is one-to-many
  * invoice has `client_id`
  * `inner join invoice on invoice.client_id = client.id`
  * `left outer join` if you want clients that have no invoices in the list

# Music Database

Follow along with https://github.com/jugonzal/lhl-lectures/blob/master/w4d1-sql-intro

* load `seed.sql`
* `echo "create database w4d1;" | psql -h localhost -p 5432 -U postgres`
* `psql -h localhost -p 5432 -U postgres -d w4d1 < seed.sql`
* `psql -h localhost -p 5432 -U postgres -d w4d1`
  * HeidiSQL and other GUIs

## [queries.sql](https://raw.githubusercontent.com/jugonzal/lhl-lectures/master/w4d1-sql-intro/queries.sql)

* all tracks, with artist name and album name
  * same as above (`where` instead of `join`)
* all artists with tags
  * same as above (`group` and `string_agg`)
* number of albums per artist
  * who have more than 2 albums, desc. frequency
  * that were released after the year 2000
* average number of tracks per album
  * using implicit `join` instead of `distinct`
* average number of tracks in albums of each tag, desc. frequency
* total plays of tracks on an album
  * using `left outer join` to correct the error
  * `left` vs `right`?

# Aside: the database index
* when we ask for a subset of rows from a table, how can we get these efficiently?
* postgres automatically creates indices for primary keys (but not foreign keys)
* examine your `where` clauses and the `on` clause for the "keys" you're using to select rows from tables

# Access your data from node.js
* raw access: [pg](https://www.npmjs.com/package/pg)
  * write raw SQL queries, get one row back at a time, unpack column values yourself
  * all SQL features available to you
* query builder: [knex](https://knexjs.org/)
  * raw SQL queries or a fluent approach to building queries
* ORM: [sequelize](http://docs.sequelizejs.com/)
  * js objects that synchronize automatically* with your database
  * quick to get set up
  * can sometimes work against you later on

# Further reading
* SQL Injection: https://medium.com/intrinsic/common-node-js-attack-vectors-sql-injection-b8b65ca78b68
* More on Indexes: https://devcenter.heroku.com/articles/postgresql-indexes
* Another W4D1 lecture: https://github.com/jensen/sqlintro-notes/
