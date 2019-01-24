###### tags: `SQL`、`SELECT`、`CH09`
# SELECT語法
### 聚合函數指的是 AVG()、COUNT()、MAX()、MIN()、SUM() 等這些內建函數。
<br>

> ### 基本語法
```
SELECT * FROM 書籍 
```
<br>

> ### CAST、AS、numeric
```
SELECT 書籍名稱, CAST(價格 * 0.8 AS numeric(4, 0) )  AS  折扣價
FROM 書籍
```
*  CAST => 轉型(括弧的值)
*  AS => 把ooo當成xxx
*  numeric(4,0) => 小數點左邊可以有4位，小數點右邊0位
  ex: 1234.567 as numeric(4,0) ==> 1234
<br>

> ### LOWER
```
SELECT '大家好' , 3+5 , LOWER('ABC')
```
* LOWER ==> 轉型(大寫轉成小寫)
<br>

> ### DISTINCT
```
SELECT DISTINCT 出版公司 FROM 書籍 
```
* DISTINCT ==> 搜尋出不相同的值 (相同的值自動過濾)
<br>

> ### TOP 2
```
SELECT TOP 2 * FROM 書籍
```
* TOP 2 ==> 搜尋前兩筆的資料
<br>

> ### TOP 30 PERCENT
```
SELECT TOP 30 PERCENT * FROM 書籍
```
* TOP 30 PERCENT ==> 搜尋前百分之三十筆的資料
<br>

> ### TOP 3 WITH TIES、ORDER BY
```
SELECT TOP 3 WITH TIES * FROM 書籍 ORDER BY 價格
```
* TOP 3 WITH TIES ==> 搜尋前三筆的資料，如果比對數值相同，則一併顯示
  EX:(列出前三名，但第三名與第四名的成績相同，WITH TIES會一同列出)
* ORDER BY ==> 排序(預設皆為遞增)
<br>

> ### IDENTITYCOL、ROWGUIDCOL
```
SELECT IDENTITYCOL, ROWGUIDCOL FROM 書籍 
```
* IDENTITYCOL ==> 回傳識別資料行
* ROWGUIDCOL ==> 回傳識別資料行
<br>

> ### JOIN
```
SELECT 企劃書籍.編號, 名稱, 價錢
FROM   企劃書籍 JOIN 企劃書籍預定價
       ON 企劃書籍.編號 = 企劃書籍預定價.編號 
```
* JOIN ==> 利用不同資料表之間欄位的關連性來結合多資料表
<br>

> ### WHERE
```
SELECT 企劃書籍.編號, 名稱, 價錢
FROM  企劃書籍, 企劃書籍預定價
WHERE 企劃書籍.編號 = 企劃書籍預定價.編號
```
* WHERE ==> 進一步在 SELECT 查詢語句使用 WHERE 關鍵字搭配運算子來取出 "符合條件" 的紀錄值
<br>

> ### LEFT JOIN
```
SELECT 旗.產品名稱 AS 旗旗公司產品名稱, 旗.價格 , 
             標.產品名稱 AS 標標公司產品名稱, 標.價格
FROM   旗旗公司 AS 旗  LEFT JOIN  標標公司 AS 標 
             ON  旗.產品名稱 = 標.產品名稱
```
* LEFT JOIN ==> 用來建立左外部連接，查詢的 SQL 敘述句 LEFT JOIN 左側資料表 (table_name1) 的所有記錄都會加入到查詢結果中，即使右側資料表 (table_name2) 中的連接欄位沒有符合的值也一樣。
<br>

> ### RIGHT JOIN
```
SELECT 旗.產品名稱 AS 旗旗公司產品名稱, 旗.價格 ,
             標.產品名稱 AS 標標公司產品名稱, 標.價格 
FROM    旗旗公司 AS 旗  RIGHT JOIN  標標公司 AS 標
            ON  旗.產品名稱 = 標.產品名稱
```
* RIGHT JOIN ==> 用來建立右外部連接，查詢的 SQL 敘述句 RIGHT JOIN 右側資料表 (table_name2) 的所有記錄都會加入到查詢結果中，即使左側資料表 (table_name2) 中的連接欄位沒有符合的值也一樣。
<br>

> ### FULL JOIN
```
SELECT 旗.產品名稱 AS 旗旗公司產品名稱, 旗.價格 , 
             標.產品名稱 AS 標標公司產品名稱, 標.價格 
FROM   旗旗公司 AS 旗  FULL JOIN  標標公司 AS 標 
            ON 旗.產品名稱 = 標.產品名稱 
```
* FULL JOIN ==> 為 LEFT JOIN 與 RIGHT JOIN 的聯集，它會返回左右資料表中所有的紀錄，不論是否符合連接條件。
<br>

> ### CROSS JOIN
```
SELECT 旗.產品名稱 AS 旗旗公司產品名稱, 旗.價格 ,
             標.產品名稱 AS 標標公司產品名稱, 標.價格 
FROM   旗旗公司 AS 旗  CROSS JOIN  標標公司 AS 標 
```
* CROSS JOIN ==> 兩個資料表在結合時，不指定任何條件，即將兩個資料表中所有的可能排列組合出來，以下例而言 CROSS JOIN 出來的結果資料列數為 3×5=15 筆，因此，當有 WHERE、ON、USING 條件時不建議使用。
<br>

> ### DATEPART、SUM、GROUP BY、ORDER BY、getdate()
```
SELECT 客戶名稱 , 
              DATEPART(MONTH, 日期) AS 月份 ,
              SUM(數量) AS 出貨數量
FROM   出貨記錄
GROUP BY 客戶名稱, DATEPART(MONTH, 日期)
ORDER BY 客戶名稱, DATEPART(MONTH, 日期)
select getdate()
select DATEPART(dy, '2018-1-2')
```
* DATEPART ==> 搜尋時間之條件
  引數網址:https://docs.microsoft.com/zh-tw/sql/t-sql/functions/datepart-transact-sql?view=sql-server-2017
* SUM ==> 加總
* GROUP BY ==> 將查詢結果中特定欄位值相同的資料分為若干個群組，而每一個群組都會傳回一個資料列
* getdate() ==> 取時間
  ex:取年 ==> DatePart(year, getdate())
  ex:取月 ==>DatePart(month, getdate())
  ex:取日 ==>DatePart(day, getdate())
 <br>

> ### CUBE
```
SELECT  客戶名稱, 書名, SUM(數量) AS 總數量 
FROM     出貨記錄 
GROUP BY  CUBE (書名, 客戶名稱)
```
* CUBE ==> 資料格所組成，而依據量值群組和維度來進行組織 
  <br>

> ### ROLLUP
```
SELECT 客戶名稱, 書名, SUM(數量) AS 總數量 
FROM  出貨記錄
GROUP BY ROLLUP(客戶名稱, 書名)
```
* ROLLUP ==> 計算資料數量

  ![](https://i.imgur.com/BBiNXYn.jpg)
<br> 

> ### HAVING
```
SELECT   客戶名稱, 書名, SUM(數量) AS 總數量 
FROM      出貨記錄
GROUP BY  客戶名稱, 書名 
HAVING SUM(數量) >= 6 
```
* HAVING ==> 取代 WHERE 搭配聚合函數 (aggregate function) 進行條件查詢，因為 WHERE 不能與聚合函數一起使用。
<br>

> ### OFFSET、FETCH
```
SELECT *
FROM 出貨記錄
ORDER BY 客戶名稱 DESC, 數量 ASC
OFFSET 3 ROWS FETCH NEXT 4 ROWS ONLY
```
* OFFSET 3 ROWS ==> 跳過前3筆資料
* FETCH NEXT 4 ROWS ONLY ==> 只查詢四筆資料



