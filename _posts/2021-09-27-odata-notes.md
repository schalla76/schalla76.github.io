---
layout: post
title: "Odata Notes"
date: 2021-09-27
categories: js, postman
---

## Introduction

- Its standard for REST APIs.
- Its adapted by Microsoft and [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=odata)

## OData Uri's

### Odata URI structure

- [Service Root]/[Resource Name]/[Query Options]
- Query Options can be filter, select etc

### GET service document

| Key     | Value                        |
| ------- | ---------------------------- |
| Method  | GET                          |
| Headers |                              |
| Url     | http://localhost:5810/odata/ |
| Body    |                              |

### GET metadata document

| Key     | Value                                 |
| ------- | ------------------------------------- |
| Method  | GET                                   |
| Headers |                                       |
| Url     | http://localhost:5810/odata/$metadata |
| Body    |                                       |

### GET People

| Key     | Value                              |
| ------- | ---------------------------------- |
| Method  | GET                                |
| Headers | Accept: application/json           |
| Url     | http://localhost:5810/odata/People |
| Body    |                                    |

### GET People by ID

| Key     | Value                                 |
| ------- | ------------------------------------- |
| Method  | GET                                   |
| Headers | Accept: application/json              |
| Url     | http://localhost:5810/odata/People(1) |
| Body    |                                       |

### GET People full metadata

| Key     | Value                                        |
| ------- | -------------------------------------------- |
| Method  | GET                                          |
| Headers | Accept: application/json;odata.metadata=full |
| Url     | http://localhost:5810/odata/People           |
| Body    |                                              |

### GET People minimal metadata

| Key     | Value                                           |
| ------- | ----------------------------------------------- |
| Method  | GET                                             |
| Headers | Accept: application/json;odata.metadata=minimal |
| Url     | http://localhost:5810/odata/People              |
| Body    |                                                 |

### GET People no metadata

| Key     | Value                                        |
| ------- | -------------------------------------------- |
| Method  | GET                                          |
| Headers | Accept: application/json;odata.metadata=none |
| Url     | http://localhost:5810/odata/People           |
| Body    |                                              |

### GET Attribute

| Key     | Value                                       |
| ------- | ------------------------------------------- |
| Method  | GET                                         |
| Headers | Accept: application/json                    |
| Url     | http://localhost:5810/odata/People(5)/Email |
| Body    |                                             |

### GET raw property value

| Key     | Value                                              |
| ------- | -------------------------------------------------- |
| Method  | GET                                                |
| Headers | Accept: application/json                           |
| Url     | http://localhost:5810/odata/People(1)/Email/$value |
| Body    |                                                    |

### GET collection property

| Key     | Value                                         |
| ------- | --------------------------------------------- |
| Method  | GET                                           |
| Headers | Accept: application/json                      |
| Url     | http://localhost:5810/odata/People(5)/Friends |
| Body    |                                               |

### POST Person

| Key     | Value                                                                                                                                                                    |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Method  | POST                                                                                                                                                                     |
| Headers | Accept: application/json;Content-Type: application/json                                                                                                                  |
| Url     | http://localhost:5810/odata/People                                                                                                                                       |
| Body    | `{ "@odata.type":"AirVinyl.Model.Person", "FirstName":"John", "LastName":"Smith", "Email": "john.smith@someprovider.com", "Gender":"Male", "DateOfBirth": "1980-01-30"}` |

### POST Person and VinylRecords

| Key     | Value                                                                                                                                                                                                                                                                   |
| ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Method  | POST                                                                                                                                                                                                                                                                    |
| Headers | Accept: application/json;Content-Type: application/json                                                                                                                                                                                                                 |
| Url     | http://localhost:5810/odata/People                                                                                                                                                                                                                                      |
| Body    | `{ "@odata.type":"AirVinyl.Model.Person", "FirstName":"Emma", "LastName":"Smith", "Email": "emma.smith@someprovider.com", "Gender":"Female", "DateOfBirth": "1989-05-16", "VinylRecords": [ { "Title":"Grace", "Artist":"Jeff Buckley", "CatalogNumber":"ABC/875" } ]}` |

### Put Person

| Key     | Value                                                                                                                                                                         |
| ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Method  | PUT                                                                                                                                                                           |
| Headers | Accept: application/json;Content-Type: application/json                                                                                                                       |
| Url     | http://localhost:5810/odata/People(3)                                                                                                                                         |
| Body    | `{ "FirstName": "Nick", "LastName": "Missorten", "DateOfBirth": "1983-05-18T00:00:00+02:00", "Gender": "Male", "NumberOfRecordsOnWishList": 23, "AmountOfCashToSpend": 2500}` |

### Patch Person

| Key     | Value                                                      |
| ------- | ---------------------------------------------------------- |
| Method  | PATCH                                                      |
| Headers | Accept: application/json;Content-Type: application/json    |
| Url     | http://localhost:5810/odata/People(3)                      |
| Body    | `{ "FirstName": "Nick", "Email": "nick@someprovider.com"}` |

### Delete Person

| Key     | Value                                                   |
| ------- | ------------------------------------------------------- |
| Method  | DELETE                                                  |
| Headers | Accept: application/json;Content-Type: application/json |
| Url     | http://localhost:5810/odata/People(3)                   |
| Body    |                                                         |

### POST association

| Key     | Value                                                   |
| ------- | ------------------------------------------------------- |
| Method  | POST                                                    |
| Headers | Accept: application/json;Content-Type: application/json |
| Url     | http://localhost:5810/odata/People(7)/Friends/$ref      |
| Body    | `{"@odata.":"http://localhost:5810/odata/People(1))"}`  |

### PUT association

| Key     | Value                                                    |
| ------- | -------------------------------------------------------- |
| Method  | PUT                                                      |
| Headers | Accept: application/json;Content-Type: application/json  |
| Url     | http://localhost:5810/odata/People(7)/Friends(1)/$ref    |
| Body    | `{"@odata.id": "http://localhost:5810/odata/People(3)"}` |

### DELETE association

| Key     | Value                                                                                         |
| ------- | --------------------------------------------------------------------------------------------- |
| Method  | DELETE                                                                                        |
| Headers | Accept: application/json;Content-Type: application/json                                       |
| Url     | http://localhost:5810/odata/People(7)/Friends/\$ref?$id=http://localhost:5810/odata/People(3) |
| Body    |                                                                                               |

## OData Query Options

- EnableQuery attribute on the action filter enables the query options

- Query options are
  - Parsed
  - Validated
  - Applied
