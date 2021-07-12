#ADSQL prob set

```sql
--１ 고객 테이블(custs)을 이용하여 다음 조건에 맞는 고객을 검색하세요.
--가) 검색 컬럼
--고객번호, 이름, 전화번호, 신용한도, 성별
--나) 조건
--① 신용한도가 5000이상 9000 이하인 고객
--② 성별 컬럼이 여성(F)이거나 NULL인 고객
--③ 위 두 조건을 모두 만족해야 합니다.

select cust_id, fname ,phone, credit_limit, gender
from custs
where credit_limit between 5000 and 9000
and (gender is null or gender = 'F');

--２ 고객 테이블(custs)과 주문 테이블(orders)을 이용하여 다음 조건의 결과를 검색하세요.
--가) 검색 컬럼
--① 고객이름(lname), 주문번호, 주문일자, 전화번호, 주문 금액
--나) 조건
--① online으로 주문한 고객

select c.lname, o.order_id, o.order_date, c.phone, o.order_total
from custs c, orders o
where c.cust_id = o.cust_id
and o.order_mode = 'online';

--３ 주문 테이블(orders)을 활용하여 아래의 조건에 맞는 결과를 검색하세요.
--가) 컬럼 검색
--① 판매사원번호, 고객번호, 주문 금액
--나) 조건
--① direct로 판매된 제품의 주문 금액의 평균 값 보다 주문 금액이 적은 경우
--② ONLINE으로 주문된 경우는 제외
--③ 위 두 조건을 모두 만족하는 조건 검색
--다) 정렬
--① 주문 금액을 내림차순 정렬

select sales_rep_id, cust_id, order_total
from orders
where order_mode != 'online'
and order_total < (select avg(order_total)
                    from orders
                    where order_mode = 'direct')
order by order_total desc;

--４ 상품 테이블(prods) 주문 테이블(orders) 주문 상세 테이블(order_items), 고객 테이블(custs)을 사용하여
--상품을 구매했다가 취소한 고객의 번호와 이름, 상품번호, 주문일자, 주문했던 수량, 주문 단가 및 금액을
--검색하세요.
--가) 컬럼 검색
--① 고객번호, 이름, 상품번호, 상품이름, 주문일자,
--주문 수량, 주문 단가, 주문 금액 (quantity*unit_price)
--나) 조건
--① 주문 취소 내역이 있는 정보 검색
--다) 정렬
--① 고객번호, 주문일자, 상품번호 컬럼으로 오름차순 정렬
--실행 결과
--CUST_ID LNAME PROD_ID PROD_NAME ORDER_DATE QUANTITY UNIT_PRICE AMT
------------ ---------- ---------- -------------------- ---------- ---------- ---------- ----------
--101 Welles 3127 LaserPro 600/6/BW 2008/03/30 44 492 21648
--101 Welles 3155 Monitor Hinge - HD 2008/03/30 62 47 2914
--158 Lyon 2293 MB - S600 2007/11/20 12 94 1128
--168 Voight 2995 SPNIX3.3 SAU 2008/05/25 8 68 544

select c.cust_id, c.lname, p.prod_id, p.prod_name
    , o.order_date, oi.quantity, oi.unit_price
    , (oi.quantity * oi.unit_price) amt
from prods p, orders o, order_items oi, custs c
where c.cust_id = o.cust_id
and o.order_id = oi.order_id
and oi.prod_id = p.prod_id
--and (oi.order_id, oi.prod_id) in (select order_id, prod_id
--                                from order_cancel)
and exists (select *
            from order_cancel
            where order_id = oi.order_id
            and prod_id = oi.prod_id)
order by c.cust_id, o.order_date, p.prod_id;

--５ 상품 테이블(prods)과 주문 상세 테이블(order_items), 주문 테이블(orders), 고객 테이블(custs)을 이용하
--여 미국에서 거주하는 고객이 주문한 상품의 번호와 이름, 주문 수량, 주문 단가, 주문 금액을 조회하세요.
--가) 검색 컬럼
--① 주문번호, 상품번호, 상품이름, 주문수량, 주문단가, 주문금액 (quantity * unit_price)
--나) 조건
--① 미국에 거주중인 고객이 주문한 정보를 검색
--다) 정렬
--① 주문금액 컬럼을 내림차순으로 정렬
--실행 결과
--ORDER_ID PROD_ID PROD_NAME QUANTITY UNIT_PRICE AMT
------------ ---------- -------------------- ---------- ---------- ----------
--2390 1948 Envoy IC/58 16 470.8 7532.8
--2390 1910 FG Stock - H 4 14 56
--2390 1912 SS Stock - 3mm 2 14 28

select o.order_id, p.prod_id, p.prod_name
    , oi.quantity, oi.unit_price
    , (oi.quantity * oi.unit_price) amt
from prods p, orders o, order_items oi, custs c
where c.cust_id = o.cust_id
and o.order_id = oi.order_id
and oi.prod_id = p.prod_id
and c.cust_id in (select cust_id
                    from custs
                    where country = 'USA')
--and exists (select *
--            from custs
--            where c.country = 'USA')
order by amt desc;
```