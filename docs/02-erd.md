# 📄 ERD 및 DB 모델링 문서

## ✅ 개요

본 문서는 e-커머스 상품 주문 서비스의 데이터베이스 설계를 위한 ERD와 Entity 간 관계를 설명합니다. 설계 목적은 기능 간 데이터 흐름을 구조적으로 정의하고, 주문 처리 및 통계 집계를 위한 기반을 마련하는 것입니다.

---

## ✅ 주요 Entity 목록 및 설명

| 엔티티       | 설명                              |
| ------------ | --------------------------------- |
| `User`       | 사용자 정보 및 잔액 관리          |
| `Product`    | 상품 정보 및 재고 관리            |
| `Order`      | 주문 요청 정보 (1\:N - OrderItem) |
| `OrderItem`  | 주문 상세 항목 (상품별 수량)      |
| `Coupon`     | 발급 가능한 선착순 쿠폰 정보      |
| `UserCoupon` | 사용자에게 발급된 쿠폰 정보       |

---

## ✅ ERD 설계 (텍스트 표현)

```
User
- id (PK)
- name
- balance
- created_at

Product
- id (PK)
- name
- price
- stock
- created_at

Order
- id (PK)
- user_id (FK -> User)
- total_price
- discounted_price
- created_at

OrderItem
- id (PK)
- order_id (FK -> Order)
- product_id (FK -> Product)
- quantity
- unit_price

Coupon
- id (PK)
- name
- discount_amount
- total_quantity
- issued_count
- created_at

UserCoupon
- id (PK)
- user_id (FK -> User)
- coupon_id (FK -> Coupon)
- is_used (boolean)
- issued_at
- used_at (nullable)
```

---

## ✅ Entity 간 관계 요약

-   `User 1 : N Order`
-   `Order 1 : N OrderItem`
-   `Product 1 : N OrderItem`
-   `User 1 : N UserCoupon`
-   `Coupon 1 : N UserCoupon`

---

## ✅ 확장 고려사항

-   인기 상품 조회를 위한 **OrderItem + created_at 인덱스** 추가
-   `Order`와 `OrderItem`은 cascade delete 고려
-   쿠폰은 issued_count 증가 시 동시성 고려 필요
-   추후 `PaymentLog`, `Refund` 등 서브 도메인 추가 가능성 열어둘 것

---

## 📌 다음 문서

-   [요구사항 분석 문서](./01-requirements.md)
-   [API 명세 문서 (작성 예정)](./03-api-spec.md)
