# Forum API Documentation and Reference

[TOC]

## database connection

**Provider**: Npgsql

```c#
var connString = "Host=myserver;Username=mylogin;Password=mypass;Database=mydatabase";
await using var conn = new NpgsqlConnection(connString);

// Retrieve environment variable
var value = Environment.GetEnvironmentVariable("KEY");
```

### Connection Environment Variables

| Key           | Description                                     |
| ------------- | ----------------------------------------------- |
| pgsqlusername | username for role that connection will use      |
| pgsqlpassword | password for role that connection will use      |
| pgsqlhost     | host/url for database                           |
| pgsqldatabase | database title, as it is recognized by the dbms |

## Containers

**PSQL Database**: 127.0.0.1:7050

Docker configuration under ./Database,

SQL files used to init database container.

**ASP.NET Server**: 127.0.0.1:7051

### API Logging

**Log API Requests and Responses**

Successfull Request:

timestamp, HTTP method, url, body, headers, response code, response headers, response body text.

Unsuccessful Requst:

timestamp, HTTP method, url, request body, request headers, reponse code, response headers, response sbody test

## Forum Web Api

**Authorization**: HTTP Request Header 'Authorization': 'Bearer TheJWTSecurityKey'

**User Credentials**: Email and a password are used to request authorization, located within the HTTP request body.

**Response Formats**: JSON



### Users

#### POST /users/authenticate => Authentication, JWT

- Requires Authentication: No

  HTTP Request Body:


| Name     | Required | Description                                                                          | Default Value | Example                                |
| -------- | -------- | ------------------------------------------------------------                         | ------------- | -------------------------------------- |
| Email    | required | Email associated with an existing user account                                       |               | {... , "Email": "Example@Email"}       |
| Password | required | Plain text password associated with an existing user account. In HTTP response body. |               | {... , "Password": "Example Password"} |

#### POST /users => create a new users, signup

- Requires Authentication: No

  HTTP Request Body:

| Name     | Required | Description                                                                          | Default Value | Example                                |
| -------- | -------- | ------------------------------------------------------------                         | ------------- | -------------------------------------- |
| Email    | required | Email associated with an existing user account                                       |               | {... , "Email": "Example@Email"}       |
| Password | required | Plain text password associated with an existing user account. In HTTP response body. |               | {... , "Password": "Example Password"} |

#### GET /users => retrieve list of users, and public user information

#### GET /users/:id => [Authenticated] retrieve user information

#### PUT /users/:id => [Authenticated] update (full) user info
#### PATCH /users/:id => [Authenticated] partial update
#### DELETE /users/:id => [Authenticated] delete user



### Threads

#### POST /threads => create new thread entity
#### GET /threads => retrieve list of threads
#### DELETE /threads/:id => delete thread (and associated posts)
#### GET /threads/:id => retrieve posts for thread



### Posts

#### POST /threads/:id/posts => insert/create new post within thread
#### PUT /threads/:id/posts/:id => update post
#### DELETE /thread/:id/posts:id => delete post
