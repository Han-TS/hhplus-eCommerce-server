# 📄 API 명세 문서

> 서비스명: e-커머스 상품 주문 서비스
> 작성일: 2025.05.29

---

## ✅ 공통 사항

-   **Base URL:** `/api`
-   **Content-Type:** `application/json`
-   **인증:** 향후 적용 가능 (현재는 생략)

---

## 1. 📥 잔액 충전 및 조회 API

### POST `/balance/charge`

| 필드   | 타입   | 설명      |
| ------ | ------ | --------- |
| userId | number | 사용자 ID |
| amount | number | 충전 금액 |

**Response:**

```json
{
    "balance": 15000
}
```

---

### GET `/balance/:userId`

**Response:**

```json
{
    "userId": 1,
    "balance": 20000
}
```

---

## 2. 📦 상품 조회 API

### GET `/products`

**Response:**

```json
[
  {
    "id": 1,
    "name": "노트북",
    "price": 1200000,
    "stock": 3
  },
  ...
]
```

---

## 3. 💳 주문 / 결제 API

### POST `/orders`

| 필드     | 타입              | 설명                |
| -------- | ----------------- | ------------------- |
| userId   | number            | 사용자 ID           |
| items    | array             | 상품 ID와 수량 목록 |
| couponId | number (nullable) | 쿠폰 ID (선택)      |

```json
{
    "userId": 1,
    "items": [
        { "productId": 1, "quantity": 2 },
        { "productId": 2, "quantity": 1 }
    ],
    "couponId": 3
}
```

**Response:**

```json
{
    "orderId": 15,
    "totalPrice": 50000,
    "discountedPrice": 45000,
    "status": "PAID"
}
```

---

## 4. 🎫 선착순 쿠폰 API

### POST `/coupons/claim`

| 필드   | 타입   | 설명      |
| ------ | ------ | --------- |
| userId | number | 사용자 ID |

**Response:**

```json
{
    "couponId": 3,
    "discountAmount": 5000
}
```

---

### GET `/coupons/:userId`

**Response:**

```json
[
  {
    "couponId": 3,
    "discountAmount": 5000,
    "isUsed": false
  },
  ...
]
```

---

## 5. 📊 인기 상품 조회 API

### GET `/products/popular`

**Response:**

```json
[
  {
    "productId": 1,
    "name": "무선 이어폰",
    "soldCount": 28
  },
  ...
]
```

---

## 📌 오류 응답 형식 (예시)

```json
{
    "statusCode": 400,
    "message": "잔액이 부족합니다.",
    "error": "Bad Request"
}
```

---

## 📚 참고 문서

-   [요구사항 분석 문서](./01-requirements.md)
-   [ERD 및 모델링 문서](./02-erd.md)
