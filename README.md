# Sleep Tracker API Documentation
### API link
<https://sleep-tracker-backend.herokuapp.com/>

## Routes
## /auth Endpoints
### auth/register
#### POST
* Requires an object containing the new users information
* Returns a JSON object  
    `{"message": "User successfully created"}`

|Parameters| Description|
|:-------|:--------------|
|username|_string_, _required_, must be unique|
|email|_string_, _required_, must be unique| 
|password|_string_, _required_|

### auth/login
#### POST
* Requires an object containing the users information
* Stores a cookie containing the jwt
* Returns a JSON object including the jwt
    
```js
{
    token: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE2MDAwMDgxNjh9.mO0jOjng0nHo8NUVCOBbhleshdcKBDB_KKMMgpeY1ZU",
    message: `Welcome ${username}!`    
}
```

|Parameters| Description|
|:-------|:--------------|
|username|_string_, _required_, must be unique|
|password|_string_, _required_|

### auth/logout
#### GET
* Returns a status code 204 if logout successful 

## /users Endpoints
### users/current-user
#### GET
* Requires a user to be logged in 
* Returns a JSON object containing logged in users information
```js
{
    "id": "c0137caf-69db-4040-8fef-7490ff829d45",
    "username": "JohnDoe",
    "email": "email@email.com",
    "recommended_hours": 8,
    "admin": 1
}   
```

### users/current-user
#### PUT
* Requires a user to be logged in
* Requires at least one valid parameter to update in request body
* Returns a status code 204 if update successful 

|Parameters| Description|
|:-------|:--------------|
|admin|_boolean_, _optional_|
|username|_string_, _optional_|
|password|_string_, _optional_|

### users/current-user
#### DELETE
* Requires a user to be logged in 
* Returns a status code of 204 if delete user successful

## /admin Endpoints
### admin/users
#### GET
* Requires an admin user to be logged in 
* Returns an array containing all users information
```js
{
    "id": "c0137caf-69db-4040-8fef-7490ff829d45",
    "username": "JohnDoe",
    "email": "email@email.com",
    "recommended_hours": 8,
    "admin": 1
}   
```

### admin/users/:id
#### GET
* Requires an admin user to be logged in 
* Requires a user Id
* Returns a JSON object containing a users information by id
```js
{
    "id": "c0137caf-69db-4040-8fef-7490ff829d45",
    "username": "JohnDoe",
    "email": "email@email.com",
    "recommended_hours": 8,
    "admin": 1
}   
```

### admin/users/:id
#### DELETE
* Requires an admin user to be logged in 
* Requires a user Id to delete
* Returns a status code of 204 if delete user successful

## sleep Endpoints 
### day/current-user
#### POST
#### creates a new sleep log for current day
* Requires user to be logged in
* Returns a JSON object containing newly created sleep log data

|Parameters| Description|
|:-------|:--------------|
|bedtime|_string_, _required_, format: "2020-09-17 23:00:00"|
```js
{
    "id": "eed6c951-b048-4274-a61e-bba8c8eb29b5",
    "bedtime": "2020-09-17T23:00:00.000Z",
    "date": "2020-09-18T04:00:00.000Z",
    "wake_time": null,
    "total_hours_slept": null,
    "average_quality": 0
}   
```
### day/:id
#### PUT
#### updates a sleep day log by id
* Requires user to be logged in
* Requires a valid sleep log id for endpoint :id
* Returns a JSON object containing updated sleep log data

|Parameters| Description|
|:-------|:--------------|
|wake_time|_string_, _required_, format: "2020-09-17 23:00:00"|
|wake_score|_integer (between 1-4)_, _optional_|
|day_score|_integer (between 1-4)_, _optional_|
|bedtime_score|_integer (between 1-4)_, _optional_|

```js
{
    "id": "844a7234-6a86-4241-aef5-3f562d7ae494",
    "date": "2020-09-18T04:00:00.000Z",
    "bedtime": "2020-09-17T23:00:00.000Z",
    "wake_time": "2020-09-18T06:00:00.000Z",
    "total_hours_slept": 7,
    "average_quality": 2,
    "wake_score": 2,
    "day_score": 1,
    "bedtime_score": 3
} 
```

### day/all/current-user
#### GET

* Requires user to be logged in
* Returns an array containing all sleep day log data for a user by user Id
 ordered by date

```js
[
    {
        "id": "844a7234-6a86-4241-aef5-3f562d7ae494",
        "date": "2020-09-18T04:00:00.000Z",
        "bedtime": "2020-09-17T23:00:00.000Z",
        "wake_time": null,
        "total_hours_slept": null,
        "average_quality": 0,
        "wake_score": 0,
        "day_score": 0,
        "bedtime_score": 0
    },
    {
        "id": "844a7234-6a86-4241-aef5-3f562d7ae494",
        "date": "2020-09-18T04:00:00.000Z",
        "bedtime": "2020-09-17T23:00:00.000Z",
        "wake_time": "2020-09-18T06:00:00.000Z",
        "total_hours_slept": 7,
        "average_quality": 2,
        "wake_score": 2,
        "day_score": 1,
        "bedtime_score": 3
    }
]
```
### day/latest/current-user
#### GET
* Requires user to be logged in
* Returns a JSON object containing most recent sleep day log data for a user by
 user id ordered by date

```js

    {
        "id": "844a7234-6a86-4241-aef5-3f562d7ae494",
        "date": "2020-09-18T04:00:00.000Z",
        "bedtime": "2020-09-17T23:00:00.000Z",
        "wake_time": null,
        "total_hours_slept": null,
        "average_quality": 0,
        "wake_score": 0,
        "day_score": 0,
        "bedtime_score": 0
    }

```
### day/current-user/?date={date}
#### GET
* Requires user to be logged in
* Requires a date string in the format 2020-09-18
* Returns a JSON object containing sleep day log data for a user by
  date query

```js

    {
        "id": "844a7234-6a86-4241-aef5-3f562d7ae494",
        "date": "2020-09-18T04:00:00.000Z",
        "bedtime": "2020-09-17T23:00:00.000Z",
        "wake_time": null,
        "total_hours_slept": null,
        "average_quality": 0,
        "wake_score": 0,
        "day_score": 0,
        "bedtime_score": 0
    }

```
### day/:id
#### DELETE
* Requires user to be logged in
* Requires a valid sleep log id for endpoint :id
* Returns a status code of 204 if sleep day log deleted successfully

### week/all/current-user
#### GET

* Requires user to be logged in
* Returns an array containing all sleep week log data for a user by
 user Id
 ordered by date

```js
[
     {
        "id": "d78c52f1-7098-4990-85c7-9c34c8de5ce8",
        "week_of_year": "38/2020",
        "average_hours_slept": 7,3,
        "average_quality": 3,
        "users_id": "e993312b-227d-4f1c-8e50-6f14908a8dfd"
    }
]
```

### /week/?date={date}
#### GET
* Requires user to be logged in
* Requires a date string in the format 9-19-2020
* Returns a JSON object containing sleep week log data for a user by
  date query
  
```js
[
     {
        "id": "d78c52f1-7098-4990-85c7-9c34c8de5ce8",
        "week_of_year": "38/2020",
        "average_hours_slept": 7,3,
        "average_quality": 3,
        "users_id": "e993312b-227d-4f1c-8e50-6f14908a8dfd"
    }
]
```

### month/all/current-user
#### GET

* Requires user to be logged in
* Returns an array containing all sleep month log data for a user by
 user Id
 ordered by date

```js
[
     {
        "id": "d78c52f1-7098-4990-85c7-9c34c8de5ce8",
        "month_of_year": "38/2020",
        "average_hours_slept": 7,3,
        "average_quality": 3,
        "users_id": "e993312b-227d-4f1c-8e50-6f14908a8dfd"
    }
]
```

### /month/?date={date}
#### GET
* Requires user to be logged in
* Requires a date string in the format 09-19-2020
* Returns a JSON object containing sleep month log data for a user by
  date query
  
```js
[
     {
        "id": "d78c52f1-7098-4990-85c7-9c34c8de5ce8",
        "month_of_year": "38/2020",
        "average_hours_slept": 7,3,
        "average_quality": 3,
        "users_id": "e993312b-227d-4f1c-8e50-6f14908a8dfd"
    }
]
```
