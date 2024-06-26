# Event API documentation

TODO: Write a quick intro

## Create an Event

This endpoint allows you to create a new event in the system. To create an event, you must provide details such as the start time, end time, title, memo, type, teacher code, classroom code, and promo code.

### HTTP Request

`POST /event/create`

### Headers

- `Content-Type: application/json`
- `Authorization: Bearer <your_access_token>`

### Request Body

| Field           | Type   | Description                               |
|-----------------|--------|-------------------------------------------|
| `start`         | String | Start time of the event (ISO 8601 format) |
| `end`           | String | End time of the event (ISO 8601 format)   |
| `title`         | String | Title of the event                        |
| `memo`          | String | Additional notes for the event            |
| `type`          | String | Type of the event                         |
| `teacher_code`  | String | Unique identifier for the teacher         |
| `classroom_code`| String | Unique identifier for the classroom       |
| `promo_code`    | String | Promotion code associated with the event  |

### cURL Example

```bash
curl --location '127.0.0.1:5000/event/create' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer XXX' \
--data '{
  "start": "2023-11-11T11:00:00+00:00",
  "end": "2023-11-11T11:30:00+00:00",
  "title": "Test de résa",
  "memo": "réunion projet",
  "type": "Pro",
  "teacher_code": "9882",
  "classroom_code": "AGR_A016",
  "promo_code": "2-L3IN"
}'
```

### Response

The response will include details of the created event along with a unique event ID.

#### Example Success Response

```json
{
    "event_code": "de8583b7",
    "message": "Event created successfully"
}
```

#### Example Error Response

```json
{
    "error": "Teacher not avaible"
}
```
---

## Get Events by Teacher

This endpoint retrieves all events associated with a specific teacher. It requires the teacher's unique code and an authorization token.

### HTTP Request

`GET /event/get/teacher/{TEACHER_CODE}`

### Headers

- `Authorization: Bearer {YOUR_TOKEN}`

### URL Parameters

| Parameter      | Type   | Description                               |
|----------------|--------|-------------------------------------------|
| `TEACHER_CODE` | String | Unique identifier for the teacher whose events you want to retrieve |

### cURL Example

```bash
curl --location 'http://127.0.0.1:5000/event/get/teacher/{TEACHER_CODE}' \
--header 'Authorization: Bearer {YOUR_TOKEN}'
```

### Response

The response will include an array of events associated with the specified teacher. Each event object in the array contains details such as the event code, start and end times, title, memo, type, and potentially classroom and promo codes.

#### Example Success Response

```json
{
    "results": [
        {
            "code": null,
            "end": "2023-10-11T11:30:00+00:00",
            "favori": "",
            "memo": null,
            "start": "2023-10-11T11:00:00+00:00",
            "title": "Matière : CMI 1 : INITIATION RECHERCHE\nEnseignant : XXXXXX\nTD : L1 S1, MI - I 6 (CMI)\nSalle : S8 = C 030\nType : TD\nMémo : CMI UNIQUEMENT\n",
            "type": "TD"
        },
        ...
        {
            "code": "42000c5c",
            "end": "2024-11-11T11:30:00+00:00",
            "memo": "réunion projet",
            "start": "2024-11-11T11:00:00+00:00",
            "title": "Matière : Test de résa\nEnseignant : XXXXXXX\nSalle : a016 v ( gt )\nPromotion : L3 INFORMATIQUE\nType : Pro\nMémo : réunion projet",
            "type": "Pro"
        }
    ]
}
```

---

## Get Events by Classroom

Retrieve a list of events scheduled in a specific classroom. This endpoint is useful for managing classroom bookings and ensuring there are no scheduling conflicts.

### HTTP Request

`GET /event/get/classroom/{CLASSROOM_CODE}`

### Headers

- `Authorization: Bearer {YOUR_TOKEN}`

### URL Parameters

| Parameter        | Type   | Description                                      |
|------------------|--------|--------------------------------------------------|
| `CLASSROOM_CODE` | String | Unique identifier for the classroom of interest |

### cURL Example

```bash
curl --location 'http://127.0.0.1:5000/event/get/classroom/{CLASSROOM_CODE}' \
--header 'Authorization: Bearer {YOUR_TOKEN}'
```

### Response

The response is a JSON object containing an array of events associated with the specified classroom. Each event includes information such as its start and end times, title, memo, and type, which may be useful for room scheduling and management.

#### Example Success Response

```json
{
    "results": [
        {
            "code": null,
            "end": "2023-09-14T10:00:00+00:00",
            "favori": "",
            "memo": null,
            "start": "2023-09-14T08:00:00+00:00",
            "title": "Matière : PREPARATION AU PROJET PROFESS\nPromotion : M1 HYDROGEOLOGIE, SOL ET ENVIRONNEMENT (HSE)\nSalle : A016 V ( GT )\nType : CM/TD\nMémo : Maison de l'eau",
            "type": "CM"
        },
        ...
        {
            "code": "42000c5c",
            "end": "2024-11-11T11:30:00+00:00",
            "memo": "réunion projet",
            "start": "2024-11-11T11:00:00+00:00",
            "title": "Matière : Test de résa\nEnseignant : XXXXXXX\nSalle : a016 v ( gt )\nPromotion : L3 INFORMATIQUE\nType : Pro\nMémo : réunion projet",
            "type": "Pro"
        }
    ]
}
```



---

## Get Events by Promotion

This endpoint provides a comprehensive list of events associated with a specific academic promotion, such as a class or cohort. It's particularly useful for students, faculty, and staff looking to get an overview of scheduled academic and extracurricular activities for a particular group.

### HTTP Request

`GET /event/get/promotion/{PROMOTION_CODE}`

### Headers

- `Authorization: Bearer {YOUR_TOKEN}`

### URL Parameters

| Parameter        | Type   | Description                                  |
|------------------|--------|----------------------------------------------|
| `PROMOTION_CODE` | String | Unique identifier for the promotion of interest |

### cURL Example

```bash
curl --location 'http://127.0.0.1:5000/event/get/promotion/{PROMOTION_CODE}' \
--header 'Authorization: Bearer {YOUR_TOKEN}'
```

### Response

The response includes a JSON object containing an array of events related to the specified promotion. Each event in the array provides details about the event's timing, location, nature, and any additional notes.

#### Example Success Response

```json
{
    "results": [
        {
            "code": "2-L3IN",
            "end": "2023-09-21T17:00:00+00:00",
            "favori": "",
            "memo": null,
            "start": "2023-09-21T15:30:00+00:00",
            "title": "Matière : UEO SUAPS  : SPORT SEMESTRE 5\nPromotion : L3 INFORMATIQUE\n",
            "type": "UEO"
        },
        ...
        {
            "code": "42000c5c",
            "end": "2024-11-11T11:30:00+00:00",
            "memo": "réunion projet",
            "start": "2024-11-11T11:00:00+00:00",
            "title": "Matière : Test de résa\nEnseignant : XXXXXXX\nSalle : a016 v ( gt )\nPromotion : L3 INFORMATIQUE\nType : Pro\nMémo : réunion projet",
            "type": "Pro"
        }
    ]
}
```

### Notes

- The `code` field may be `null` for some events if they are fetched from external sources or if the event code is not applicable.
- The response includes various details about each event, potentially including custom memos, type designations (e.g., "TD" for teaching duties, "Pro" for professional events), and identifiers for related entities like classrooms or promotions.
- The response format and content might vary based on the specific implementation and data availability.