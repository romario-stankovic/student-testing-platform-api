# **Test Controller**

## **Requests**

<br>

### **Get single or multiple tests**
<br>

```GET: /api/test?by=[default | professor]&id=[number]```

Response (default):
```
{
    "testId": 1,
    "professorId": 1,
    "testName": "Test",
    "duration": 300,
    "questionCount": 20,
    "startAt": "1970-01-01T00:00:00.000Z",
    "endAt": "1970-01-01T00:00:00.000Z"
}
```

Response (professor)

```
[
    {
        "testId": 1,
        "professorId": 1,
        "testName": "Test",
        "duration": 300,
        "questionCount": 20,
        "startAt": "1970-01-01T00:00:00.000Z",
        "endAt": "1970-01-01T00:00:00.000Z"
    },
    ...
]
```

Response (error):

```
{
    "status": "Controller Error",
    "statusCode": 2001,
    "message": null
}
```

Response Codes:

| Response code | Response status | Reason |
|---|---|---|
| 400 | Bad Request | "by" query is missing or incorrect |
| 401 | Unauthorized | The user accessing this resource is not authorized |
| 403 | Forbidden | Wrong user is accessing this resource |
| 2001 | Controller Warning | There are no tests matching this query |

<br>

---

<br>

### **Get active tests**
<br>

```GET: /api/test/active```

Response (valid):
```
[
    {
        "testId": 1,
        "professorId": 1,
        "testName": "Test",
        "duration": 300,
        "questionCount": 20,
        "startAt": "1970-01-01T00:00:00.000Z",
        "endAt": "1970-01-01T00:00:00.000Z"
    },
    ...
]
```

Response (error):

```
{
    "status": "Controller Warning",
    "statusCode": 2001,
    "message": null
}
```

Response Codes:

| Response code | Response status | Reason |
|---|---|---|
| 401 | Unauthorized | The user accessing this resource is not authorized |
| 403 | Forbidden | Wrong user is accessing this resource |
| 2001 | Controller Warning | There are no active tests |

<br>

---

<br>

### **Get test questions**
<br>

```GET: /api/test/questions?id=[number]```

Response (valid):
```
[
    {
        "questionId": 1,
        "testId": 1,
        "questionText": "Text",
        "imagePath": "questions/question1.png",
        "answers": [
            {
                "answerId": 1,
                "questionId": 1,
                "answerText": "Text",
                "imagePath": null,
                "isCorrect": 0
            },
            ...
        ]
    },
    ...
]
```

Response (error):

```
{
    "status": "Controller Error",
    "statusCode": 2001,
    "message": null
}
```

Response Codes:

| Response code | Response status | Reason |
|---|---|---|
| 401 | Unauthorized | The user accessing this resource is not authorized |
| 403 | Forbidden | Wrong user is accessing this resource |
| 2001 | Controller Warning | There are no questions for this tests |

<br>

---

<br>

### **Add test**
<br>

```POST: /api/test```

Body:
```
{
    professorId: number,
    testName: string,
    duration: number,
    questionCount: number,
    startAt: ISO8601_Date,
    endAt: ISO8601_Date
}
```

Response (valid):
```
{
    "testId": 1,
    "professorId": 1,
    "testName": "Test",
    "duration": 300,
    "questionCount": 20,
    "startAt": "1970-01-01T00:00:00.000Z",
    "endAt": "1970-01-01T00:00:00.000Z"
}
```

Response (error):

```
{
    "status": "Controller Error",
    "statusCode": 2001,
    "message": null
}
```

Response Codes:

| Response code | Response status | Reason |
|---|---|---|
| 400 | Bad Request | Field validation failed |
| 401 | Unauthorized | The user accessing this resource is not authorized |
| 403 | Forbidden | Wrong user is accessing this resource |
| 1001 | Service Error | Saving to the database failed |

<br>

---

<br>

### **Update test**
<br>

``` PATCH: /api/test```

**NULL fields do not need to be specified*

Body:
```
{
    testId: number,
    professorId: number | null,
    testName: string | null,
    duration: number | null,
    questionCount: number | null,
    startAt: ISO8601_Date | null,
    endAt: ISO8601_Date | null
}
```

Response (status / error):
```
{
    "status": "Controller Error",
    "statusCode": 2001,
    "message": null
}
```

Response Codes:

| Response code | Response status | Reason |
|---|---|---|
| 0 | OK! | Update successful |
| 400| Bad Request | Field validation failed |
| 401 | Unauthorized | The user accessing this resource is not authorized |
| 403 | Forbidden | Wrong user is accessing this resource |
| 1001 | Service Error | Saving to the database failed |

<br>

---

<br>

### **Modify test questions**
<br>

```POST /api/test/questions/```

**Questions and answers with NULL IDs will be added, while those with IDs will be modified*

**If a question/answer is being added and fields are NULL, saving will fail for that specific entry*

Body:
```
{
    "testId": number,
    "questions": {
        "questionId": number | null,
        "questionText": string | null,
        "imagePath" : string | null,
        "toDelete" : boolean | null,
        "answers": {
            "answerId": number | null,
            "answerText": string | null,
            "imagePath": string | null,
            "isCorrect" : boolean | null,
            "toDelete": boolean | null
        }[]
    }[]
}
```

Response (status / error):
```
{
    "status": "OK!",
    "statusCode": 0,
    "message": null
}
```

Response Codes:
| Response code | Response status | Reason |
|---|---|---|
| 0 | OK! | Update successful |
| 400 | Bad Request | Field validation failed |
| 401 | Unauthorized | The user accessing this resource is not authorized |
| 403 | Forbidden | Wrong user is accessing this resource |
| 2001 | Controller Warning | Test does not exist |

<br>

---

<br>

### **Delete test**
<br>

```DELETE: /api/test```

Body:
```
{
    "testId": number
}
```

Response (status / error):
```
{
    "status": "OK!",
    "statusCode": 0,
    "message": null
}
```

Response Codes:
| Response code | Response status | Reason |
|---|---|---|
| 0 | OK! | Delete successful |
| 400 | Bad Request | Field validation failed |
| 401 | Unauthorized | The user accessing this resource is not authorized |
| 403 | Forbidden | Wrong user is accessing this resource |
| 1002 | Service Error | Deleting failed |