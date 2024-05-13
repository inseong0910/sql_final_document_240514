### SQL_FINAL_DOCUMENT_240514
### 내일배움캠프 SQL 개인학습 정리(csv 파일 2개 분석)
*** 

![SQL_assignment](.C:/Users/USER/Desktop/SQL_assignment.png)

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



