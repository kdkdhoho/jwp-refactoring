# 키친포스

## 요구 사항

## 용어 사전

|   한글명    |       영문명        |              설명               |
|:--------:|:----------------:|:-----------------------------:|
|    상품    |     product      |      메뉴를 관리하는 기준이 되는 데이터      |
|  메뉴 그룹   |    menu group    |           메뉴 묶음, 분류           |
|    메뉴    |       menu       |    메뉴 그룹에 속하는 실제 주문 가능 단위     |
|  메뉴 상품   |   menu product   |       메뉴에 속하는 수량이 있는 상품       |
|    금액    |      amount      |            가격 * 수량            |
|  주문 테이블  |   order table    |       매장에서 주문이 발생하는 영역        |
|  빈 테이블   |   empty table    |      주문을 등록할 수 없는 주문 테이블      |
|    주문    |      order       |         매장에서 발생하는 주문          |
|  주문 상태   |   order status   | 주문은 조리 ➜ 식사 ➜ 계산 완료 순서로 진행된다. |
| 방문한 손님 수 | number of guests | 필수 사항은 아니며 주문은 0명으로 등록할 수 있다. |
|  단체 지정   |   table group    | 통합 계산을 위해 개별 주문 테이블을 그룹화하는 기능 |
|  주문 항목   | order line item  |       주문에 속하는 수량이 있는 메뉴       |
|  매장 식사   |      eat in      |      포장하지 않고 매장에서 식사하는 것      |

### Product

- [v] 상품을 저장한다.
    - [v] 상품의 가격이 `null`이거나 0 미만이면 예외를 발생한다.
        - [v] 저장이 완료되면 저장된 상품 정보를 응답한다.
        - [v] location 헤더에 해당 상품 조회 URI를 응답한다.
- [v] 저장된 모든 상품을 조회한다.

### Order

- [v] 주문을 저장한다.
    - [v] 주문에 주문 항목이 없으면 예외를 발생한다.
    - [v] 요청된 주문에 메뉴가 DB에 저장되어 있지 않으면 예외를 발생한다.
    - [v] 요청된 주문 테이블이 DB에 저장되어 있지 않으면 예외를 발생한다.
    - [v] 주문 테이블이 비어있는 상태이면 예외를 발생한다.
    - [v] 주문 상태를 `COOKING`으로 변경한다.
    - [v] 주문 시간은 현재 시각으로 설정한다.
    - [v] 주문 항목에 주문 테이블 ID를 설정하고 DB에 저장한다.
        - [v] 저장이 완료되면 저장된 주문 정보를 응답한다.
        - [v] location 헤더에 해당 주문 조회 URI를 응답한다.
- [v] 저장된 모든 주문을 조회한다.
- [v] 주문 상태를 변경한다.
    - [v] 주문의 상태가 이미 '계산 완료'이면 예외를 발생한다.

### Menu

- [v] 메뉴를 저장한다.
    - [v] 요청된 메뉴의 가격 값이 `null`이거나 0 미만이면 예외를 발생한다.
    - [v] 요청된 메뉴의 메뉴 그룹이 저장되어 있지 않으면 예외를 발생한다.
    - [v] 요청된 메뉴의 메뉴 상품이 저장되어 있지 않으면 예외를 발생한다.
    - [v] 요청된 메뉴의 가격이 메뉴 상품 가격의 총합보다 크면 예외를 발생한다.
    - [v] 메뉴를 저장하고 메뉴 상품에 메뉴 ID를 할당한다.
        - [v] 저장이 완료되면 저장된 메뉴 정보를 응답한다.
        - [v] location 헤더에 해당 메뉴 조회 URI를 응답한다.
- [v] 저장된 모든 메뉴를 조회한다.

### MenuGroup

- [v] 메뉴 그룹을 저장한다.
    - [v] 저장이 완료되면 저장된 메뉴 정보를 응답한다.
    - [v] location 헤더에 해당 메뉴 조회 URI를 응답한다.
- [v] 저장된 모든 메뉴 그룹을 조회한다.

### Table

- [v] 주문 테이블을 저장한다.
    - [v] 저장이 완료되면 저장된 테이블 정보를 응답한다.
    - [v] location 헤더에 해당 테이블 조회 URI를 응답한다.
- [v] 모든 주문 테이블을 조회한다.
- [v] 주문 테이블의 주문 가능 상태를 변경한다.
    - [v] 주문 테이블이 DB에 저장되어 있지 않으면 예외를 발생한다.
    - [v] 주문 테이블의 TableGroupId가 Null이 아니면 예외가 발생한다.
    - [v] 주문 테이블에 담긴 주문의 상태가 `COOKING` 또는 `MEAL`인 주문이 없으면 예외를 발생한다.
- [v] 주문 테이블의 손님 수를 변경한다.
    - [v] 요청된 손님 수가 0 미만이면 예외를 발생한다.
    - [v] 요청된 주문 테이블이 DB에 저장되어 있지 않으면 예외를 발생한다.
    - [v] 변경하려는 주문 테이블이 빈 테이블이면 예외를 발생한다.

### TableGroup

- [v] 테이블 그룹을 저장한다.
    - [v] 요청된 테이블의 수가 2개 미만이면 예외를 발생한다.
    - [v] 요청된 테이블들이 DB에 저장되어 있지 않으면 예외를 발생한다.
    - [v] 요청된 모든 테이블들이 빈 테이블이 아니거나, 이미 다른 테이블 그룹이 저장되어 있으면 예외를 발생한다.
    - [v] 요청된 모든 테이블에 테이블 그룹 정보를 저장하고 주문을 등록할 수 있는 상태로 변경한다.
        - [v] 저장이 완료되면 저장된 테이블 그룹 정보를 응답한다.
        - [v] location 헤더에 해당 테이블 그룹 조회 URI를 응답한다.
- [v] 테이블 그룹을 해제한다.
    - [v] 요청된 테이블 그룹 ID로 묶인 주문 테이블들의 상태가 하나라도 `COOKING`이거나 `MEAL`이면 예외를 발생한다.
    - [v] 요청된 테이블 그룹 ID로 묶인 주문 테이블들을 빈 테이블로 만들어준다.
