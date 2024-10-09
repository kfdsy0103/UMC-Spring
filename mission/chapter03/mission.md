## 1. 홈 화면
### API Endpoint
```
GET /api/home
```

### Request Header
```
Authorization: Bearer <token>
```

### Response Body
```json
{
  "success": "true",
  "data": {
    "point": "999999",
    "count": "7",
    "missions": [
      {
        "id": "1",
        "restaurantName" : "인하마라탕",
        "content" : "12000원 이상 식사하기",
        "point" : "500",
        "deadline" : "7"
      },
      {
        "id": "2",
        "restaurantName" : "인하반점",
        "content" : "10000원 이상 식사하기",
        "point" : "500",
        "deadline" : "10"
      }
    ]
  }
}
```

## 2. 마이 페이지 리뷰 작성
### API Endpoint
```
POST /api/reviews
```

### Request Header
```
Content-Type : application/json
Authorization: Bearer <token>
```

### Request Body
```json
{
    "restaurantId" : "1",
    "content" : "맛있어요!",
    "score" : "5",
    "images" : ["image1.png", "image2.png"]
}
```

### Response Body
```json
{
  "success" : "true"
}
```

## 3. 미션 목록 조회(진행중, 진행 완료)
### API Endpoint
```
GET /api/missions
```

### Request Header
```
Authorization: Bearer <token>
```

### Query String
```
진행중 : ?region="상암동"&status="progress"
진행완료 : ?region="상암동"&status="success"
```

### Response Body
```json
{
  "success": "true",
  "data": {
    "missions": [
      {
        "id" : "3",
        "restaurantId" : "3",
        "restaurantName" : "인하식당1",
        "content" : "12000원 이상 식사하기",
        "point" : "500",
        "deadline" : "7"
      },
      {
        "id" : "4",
        "restaurantId" : "4",
        "restaurantName" : "인하식당2",
        "content" : "10000원 이상 식사하기",
        "point" : "500",
        "deadline" : "10"
      }
    ]
  }
}
```

## 4. 미션 성공 누르기
### API Endpoint
```
PATCH /api/missions/{missionId}
```

### Request Header
```
Content-Type : application/json
Authorization: Bearer <token>
```

### Request Body
```json
{
  "status" : "success"
}
```

### Response Body
```json
{
  "success": "true"
}
```

## 5. 회원 가입 하기
### API Endpoint
```
POST /api/members/signup
```

### Request Header
```
Content-Type : application/json
```

### Request Body
```json
{
  "terms" : [
    {"termId" : "1", "isAgree" : "true"},
    {"termId" : "2", "isAgree" : "true"},
    {"termId" : "3", "isAgree" : "False"},
    {"termId" : "4", "isAgree" : "False"}
  ],
  "email" : "aaa@gmail.com",
  "name" : "홍길동",
  "gender" : "남",
  "birth" : "2002-10-30",
  "address" : "인천시 미추홀구 인하로00번길 00-00",
  "foodCategories" : ["1", "4", "7"]
}
```

### Response Body
```json
{
  "success" : "true"
}
```