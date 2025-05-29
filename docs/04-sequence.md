# 📄 시퀀스 다이어그램 문서

## ✅ 개요

이 문서는 주요 기능에 대한 **시퀀스 다이어그램 텍스트 기반 흐름 설명**을 포함합니다. 실제 다이어그램 도구(PlantUML, SequenceDiagram.org 등)에서 시각화 가능하도록 구성되어 있습니다.

---

## 📌 시나리오 1: 주문 / 결제 흐름

```
사용자 → 클라이언트: 상품 선택 후 주문 요청 (상품ID, 수량, 쿠폰ID)
클라이언트 → 서버(OrdersController): POST /orders
서버 → OrderService: 주문 요청 처리
OrderService → UserRepository: 사용자 잔액 조회
OrderService → CouponRepository: 쿠폰 유효성 확인
OrderService → ProductRepository: 상품 재고 조회
OrderService → TransactionManager: 트랜잭션 시작
TransactionManager → UserRepository: 잔액 차감
TransactionManager → ProductRepository: 재고 차감
TransactionManager → OrderRepository: 주문 생성
TransactionManager → OrderItemRepository: 주문 상세 생성
TransactionManager → CouponRepository: 쿠폰 사용 처리
TransactionManager → Commit
OrderService → ExternalDataSender: 주문 데이터 전송 (Mock 처리)
OrderService → 서버: 주문 결과 반환
서버 → 클라이언트: 주문 완료 응답
```

---

## 📌 시나리오 2: 선착순 쿠폰 발급

```
사용자 → 클라이언트: 쿠폰 발급 요청
클라이언트 → 서버(CouponController): POST /coupons/claim
서버 → CouponService: 쿠폰 발급 로직 실행
CouponService → CouponRepository: 전체 수량, 발급 수 조회
CouponService → UserCouponRepository: 사용자 발급 여부 확인
CouponService → TransactionManager: 트랜잭션 시작
TransactionManager → CouponRepository: 발급 수 증가
TransactionManager → UserCouponRepository: 사용자 쿠폰 발급 생성
TransactionManager → Commit
CouponService → 서버: 쿠폰 발급 성공 반환
서버 → 클라이언트: 쿠폰 정보 응답
```

---

## 📌 시나리오 3: 인기 상품 조회

```
사용자 → 클라이언트: 인기 상품 요청
클라이언트 → 서버(ProductController): GET /products/popular
서버 → ProductService: 최근 3일 주문 수 기준 집계
ProductService → OrderItemRepository: 최근 주문 항목 조회
ProductService → ProductRepository: 상품 정보 매핑
서버 → 클라이언트: TOP 5 인기 상품 응답
```

---

## 참고 문서

-   [API 명세 문서](./03-api-spec.md)
-   [요구사항 분석](./01-requirements.md)
