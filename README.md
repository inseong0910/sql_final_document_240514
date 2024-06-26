### SQL_FINAL_DOCUMENT_240514
### 내일배움캠프 SQL 개인학습 정리(csv 파일 2개 분석)
*** 

![SQL_assignment](https://github.com/inseong0910/sql_final_document_240514/assets/88603039/509313ac-d869-4803-99e7-52dcd6e87445)

***

#### < USER 유입 및 매출 관련 현황 : 분석 계획 > 

1. USER 가입일자 분석

2. 게임 계정수 분석

3. LEVEL별 회원수 분석

4. 현재경험치 구간별 회원수 분석

5. 서버번호별 회원수 분석

6. 지역번호별 회원수 분석

7. 아이템 획득 경로별 회원수 분석

8. 아이템 이름별 회원수 분석

9. 기간별 결제금액 분석

10. 결제금액 상위 10명, 하위 10명 분석

11. 결제수단별 회원수 & 결제금액 분석

12. 한계점 
***

__1. USER 가입일자 분석__
<br/><br/>
   ① 가입일별 가입자수
<pre><code>select date, count(date)
from users 
group by date
order by date asc</code></pre>


<br/>
   ② 연도별 가입자수
<pre><code>select year(date) as year, count(year(date)) as "연도별 가입자수"
from users
group by year 
order by year asc</code></pre>


<br/>
   ③ 월별 가입자수
<pre><code>select month(date) as month, count(month(date)) as "월별 가입자수"
from users 
group by month
order by month asc</code></pre>


<br/>
   ④ 분기별 가입자수 
<pre><code>select quarter(date) as month, count(quarter(date)) as "분기별 가입자수"
from users 
group by month
order by month asc</code></pre>


***
__2. 게임 계정수 분석__
<pre><code>select count(distinct game_account_id)as "중복값 제외",
count(game_account_id) as "중복값 포함" 
from basic.users</code></pre>

***
__3. LEVEL별 회원수 분석__
<br/><br/>
(1) LEVEL별 회원수
<pre><code>select level, count(level) as "레벨별 인원수"
from users 
group by level
order by level asc</code></pre>
<br/><br/>
(2) LEVEL 구간별 회원수
<pre><code>
# 그룹1 : 레벨1~10 / 그룹2 : 레벨11~20 / ... / 그룹10 : 레벨91~100
# case when end 사용

select date, game_actor_id, level, 
	   case when level <= 10 then '레벨 1~10'
			when level <= 20 then '레벨 11~20'
			when level <= 30 then '레벨 21~30'
			when level <= 40 then '레벨 31~40'
			when level <= 50 then '레벨 41~50'
			when level <= 60 then '레벨 51~60'
			when level <= 70 then '레벨 61~70'
			when level <= 80 then '레벨 71~80'
			when level <= 90 then '레벨 81~90'
			else '레벨 91~100'
			end as levelgroup
from users 
order by level</code></pre>
***
__4. 현재경험치 구간별 회원수 분석__
<br/><br/> (1) exp 최대값, 최소값, 평균
<pre><code>#exp를 10만 단위로 나누어 그룹별 인원 수 계산
select game_actor_id, exp, 
	   case when exp <= 100000 then '그룹1'
			when exp <= 200000 then '그룹2'
			when exp <= 300000 then '그룹3'
			when exp <= 400000 then '그룹4'
			when exp <= 500000 then '그룹5'
			when exp <= 600000 then '그룹6'
			when exp <= 700000 then '그룹7'
			when exp <= 800000 then '그룹8'
			when exp <= 900000 then '그룹9'
			else '최고'
			end as '경험치 레벨'
from users</code></pre>
<br/><br/>

(2) 최소값 14, 최대값 999,972 이므로 exp를 100,000 단위로 나누어 개별 경험치 등급 표기
<pre><code>select max(exp), min(exp), avg(exp)
from users</code></pre>
<br/><br/>


(3) exp를 100,000 단위로 나누어 단위별 인원 수 계산
<pre><code>select count(case when exp <= 100000 then '그룹1' end) as '1반', 
       count(case when exp > 100000 and exp <= 200000 then '그룹2' end) as '2반',
       count(case when exp > 200000 and exp <= 300000 then '그룹2' end) as '3반',
       count(case when exp > 300000 and exp <= 400000 then '그룹2' end) as '4반',
       count(case when exp > 400000 and exp <= 500000 then '그룹2' end) as '5반',
       count(case when exp > 500000 and exp <= 600000 then '그룹2' end) as '6반',
       count(case when exp > 600000 and exp <= 700000 then '그룹2' end) as '7반',
       count(case when exp > 700000 and exp <= 800000 then '그룹2' end) as '8반',
       count(case when exp > 800000 and exp <= 900000 then '그룹2' end) as '9반',
       count(case when exp > 900000 then '그룹2' end) as '기타'
from users</code></pre>
<br/><br/>

***
__5. 서버번호별 회원수 분석__
<br/><br/> (1) 전체 데이터 살펴보기
<pre><code># 전체 데이터 살펴보기
select *
from users ;

select *
from payment ;</code></pre>

<br/><br/> (2) 서버번호별 사용자수
<pre><code># 서버번호별 유저수
select serverno, count(serverno)
from users
group by serverno
order by serverno</code></pre>

***
__6. 지역번호별 회원수 분석__
<br/><br/>
<pre><code>select zone_id, count(zone_id)
from users
group by zone_id
order by zone_id</code></pre>
***
__7. 아이템 획득 경로별 회원수 분석__
<br/><br/>
<pre><code>select etc_str1 , count(etc_str1)
from users
group by etc_str1
order by etc_str1</code></pre>
***
__8. 아이템 이름별 회원수 분석__
<br/><br/>
<pre><code>select etc_str2 , count(etc_str2)
from users
group by etc_str2
order by etc_str2</code></pre>
***
__9. 기간별 결제금액 분석__
<br/><br/> (1) 2015년 총 결제금액
<pre><code># 2015년 총 결제 금액은?(연도별)
select year(date) as "연도", sum(p.pay_amount) as "2015년 결제금액 합계"
from users u inner join payment p on u.game_account_id = p.game_account_id
where year(u.date) = 2015
group by year(u.date)</code></pre>

<br/><br/> (2) 연도별 총 결제금액
<pre><code># 연도별 총 결제 금액은?
select year(date) as "연도", sum(p.pay_amount) as "연도별 결제금액 합계"
from users u inner join payment p on u.game_account_id = p.game_account_id
group by year(u.date)
order by year(u.date))</code></pre>

<br/><br/> (3) 2015년 1월 총 결제금액
<pre><code># 2015년 1월 총 결제 금액
select date_format(date, '%Y-%m') as "년월", sum(pay_amount) as "총 결제금액"
from users u inner join payment p on u.game_account_id = p.game_account_id
where date_format(date, '%Y-%m') = '2015-01'
group by date_format(date, '%Y-%m')</code></pre>

<br/><br/> (4) 월별 총 결제금액 조회 
<pre><code># 월별 총 결제금액 조회
select date_format(date, '%Y-%m') as "년월", sum(pay_amount) as "총 결제금액"
from users u inner join payment p on u.game_account_id = p.game_account_id
group by date_format(date, '%Y-%m')
order by date_format(date, '%Y-%m')</code></pre>

<br/><br/> (5) 한 달 중 결제금액이 가장 많은 날짜는? (1일~31일) 
<pre><code># 매월 중 결제 금액이 가장 많은 날짜는? 
select date_format(date, '%d') as "날짜", sum(pay_amount) as "총 결제금액"
from users u inner join payment p on u.game_account_id = p.game_account_id
group by date_format(date, '%d')
order by date_format(date, '%d')</code></pre>
<br/><br/>
***
__10. 결제금액 상위 10명, 하위 10명 분석__
<br/><br/> (1) 결제금액 상위 10명 
<pre><code># 결제금액 상위 10명은 누구인가?
select ip_addr, date, u.game_account_id, serverno, pay_amount
from users u inner join payment p on u.game_account_id = p.game_account_id
order by pay_amount desc limit 10</code></pre>

<br/><br/> (2) 결제금액 하위 10명 
<pre><code># 결제금액 하위 10명은 누구인가?
select ip_addr, date, u.game_account_id, serverno, pay_amount
from users u inner join payment p on u.game_account_id = p.game_account_id
order by pay_amount limit 10</code></pre>

***
__11. 결제수단별 회원수 & 결제금액 분석__
<br/><br/> (1) 결제수단별 사용자수  
<pre><code>select count(u.game_account_id) as "인원수", pay_type
from users u inner join payment p on u.game_account_id = p.game_account_id
group by pay_type</code></pre>

<br/><br/> (2) 결제수단별 결제금액 합계   
<pre><code>select pay_type, sum(pay_amount) as total_pay_amount
from users u inner join payment p on u.game_account_id = p.game_account_id
group by pay_type</code></pre>

***
__12. 한계점__
<br/><br/> (1) 서브쿼리 사용 부재
![subquery](https://github.com/inseong0910/sql_final_document_240514/assets/88603039/a0e13749-3a22-4f91-a8cf-d07aa6a32ed5)

<br/><br/> (2) 윈도우함수 사용 부재
![window_function](https://github.com/inseong0910/sql_final_document_240514/assets/88603039/cffc88c8-dd47-427e-9e38-585f8d552fd4)
