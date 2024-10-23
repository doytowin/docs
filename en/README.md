---
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Introduction

GoooQo is a CRUD framework in Golang based on the OQM technique.

OQM is a technique that builds database query statements from objects only, focusing on the mapping relationship between object-oriented programming languages and database query languages.

The first three Os in the name GoooQo represent the three major object concepts in the OQM technique, where `Qo` stands for `Query Object`, which is the core concept of OQM technique:

* `Entity Object` is used to map the static part in the SQL statements, such as the table name and column names;
* `Query Object` is used to map the dynamic part of the SQL statements, such as filter conditions, pagination, and sorting;
* `View Object` is used to map the static part in complex query statements, such as table names, column names, nested views, and the GROUP BY clause.

A basic query condition consists of a column name, a comparison operator, and a value. The fields in the entity correspond to the column names. When we use aliases to represent comparison operators and combine them with the fields in an entity object as suffixes, we get the fields of the query object. Therefore, we have the following formula:

$$
Entity + Suffix = Query
$$
